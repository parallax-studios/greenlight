# GREENLIGHT — Technical Architecture Document

**Lead Programmer**: BYTE
**Version**: 1.0
**Date**: 2026-02-07
**Target Platforms**: PC (Steam primary), Nintendo Switch (secondary), Mobile tablet (stretch)
**Status**: Pre-Production

---

## Executive Summary

GREENLIGHT is a turn-based publisher management sim with persistent AI studio entities, multi-year campaign progression, and complex systems interaction. The technical challenge is managing state complexity across a 10-year simulated campaign (40 quarters, 12-15 AI studios, 50+ game pitches, hundreds of intervention events) while maintaining snappy UI responsiveness and clear data visualization.

The architecture prioritizes three things:
1. **Fast iteration** — Designers need to tweak studio archetypes, intervention formulas, and era events daily during development
2. **Save state integrity** — A 10-year campaign represents 12-18 hours of player time; save corruption is catastrophic
3. **Clear data flow** — This game is 80% systems simulation, 20% presentation; the simulation layer must be completely decoupled from UI

Ship it functional. Profile before optimizing. Keep the data model dead simple.

---

## 1. Engine & Framework Recommendation

### Recommended: **Unity 2022 LTS**

**Rationale**:

This is a UI-heavy, 2D-only management sim with zero real-time requirements and minimal rendering complexity. Unity's strengths align perfectly:

- **UI Toolkit (formerly UIElements)** — Mature, performant, data-bindable UI system perfect for complex nested interfaces (pitch dossiers, portfolio matrices, milestone reviews)
- **Cross-platform export** — PC/Mac, Switch, and mobile from a single codebase with minimal platform-specific code
- **Rich editor tooling** — ScriptableObject architecture is ideal for data-driven design (studio definitions, pitch templates, intervention rules, era events)
- **Fast iteration** — Hot-reload, inspector-based editing, immediate play mode testing
- **Team familiarity** — Unity's C# ecosystem has massive community support; hiring and onboarding are straightforward

**Alternatives Considered**:

| Engine | Pros | Cons | Verdict |
|---|---|---|---|
| **Godot 4.x** | Open-source, lightweight, excellent UI system | Weaker console export (Switch requires third-party), smaller talent pool | Strong contender for PC-only, rejected for Switch requirement |
| **Unreal Engine 5** | Best-in-class rendering, UMG improving | Overkill for 2D UI-heavy game, slower iteration for data-heavy designs, C++ tax | Rejected — don't bring a howitzer to a knife fight |
| **Custom Web (React/Electron)** | Ultimate UI flexibility, web deployment potential | No console support, save system and performance risk, engine expertise required | Rejected — console platforms are non-negotiable |
| **GameMaker Studio 2** | Fast 2D prototyping | Weak data structure support for complex simulation, limited scalability | Rejected — simulation complexity exceeds GML's sweet spot |

**Decision**: Unity 2022 LTS. It's the simplest tool that solves all platform, UI, and iteration requirements.

---

## 2. Core Systems Architecture

### 2.1 High-Level System Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                      PRESENTATION LAYER                      │
│  (UI Toolkit views, input handling, audio, animations)       │
└────────────────┬────────────────────────────────────────────┘
                 │ Events (greenlight decision, intervention,
                 │         quarter advance, etc.)
                 ▼
┌─────────────────────────────────────────────────────────────┐
│                     GAME CONTROLLER                          │
│  CampaignController (turn execution, phase management)       │
│  Handles UI→Simulation commands, Simulation→UI updates       │
└────────────────┬────────────────────────────────────────────┘
                 │ Commands
                 ▼
┌─────────────────────────────────────────────────────────────┐
│                    SIMULATION LAYER                          │
│ ┌─────────────┐ ┌──────────────┐ ┌─────────────────────┐   │
│ │  Campaign   │ │  Studio      │ │  Portfolio          │   │
│ │  State      │ │  Manager     │ │  Manager            │   │
│ │             │ │              │ │                     │   │
│ │ -Year/Qtr   │ │ -Studios[]   │ │ -Prestige/Profit/   │   │
│ │ -Budget     │ │ -Trust       │ │  Populism scores    │   │
│ │ -Projects[] │ │ -Morale      │ │ -Shipped games      │   │
│ └─────────────┘ └──────────────┘ └─────────────────────┘   │
│                                                              │
│ ┌─────────────┐ ┌──────────────┐ ┌─────────────────────┐   │
│ │  Pitch      │ │  Era         │ │  Board              │   │
│ │  Generator  │ │  Engine      │ │  Evaluator          │   │
│ │             │ │              │ │                     │   │
│ │ -Concepts[] │ │ -Events[]    │ │ -Targets            │   │
│ │ -Quality    │ │ -Modifiers   │ │ -Satisfaction calc  │   │
│ │  seed calc  │ │              │ │                     │   │
│ └─────────────┘ └──────────────┘ └─────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────────┐
│                     DATA LAYER                               │
│  ScriptableObject definitions (studios, pitches, events)     │
│  JSON save/load serialization                                │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 Data Flow — Greenlight Decision Example

**Player clicks GREENLIGHT button on pitch #2 of Q1 2005**:

```
1. UI (GreenlightTableView) → Event: OnGreenlightClicked(pitchID)
2. CampaignController receives event
   → Validates: player has budget, open project slot
   → Calls: SimulationLayer.ExecuteGreenlight(pitchID)
3. SimulationLayer:
   a. PitchGenerator.ConvertPitchToProject(pitchID)
      → Creates new ProjectState with assigned studio, budget, timeline
   b. StudioManager.ModifyStudioMorale(studioID, +10)
   c. StudioManager.ModifyTrust(studioID, +5)
   d. CampaignState.DeductBudget(pitchBudget)
   e. CampaignState.AddActiveProject(newProject)
   f. BoardEvaluator.RecalculateProjections() (passive update)
4. CampaignController receives result → fires UI update events
5. Presentation Layer:
   → GreenlightTableView: play stamp animation, update budget display
   → StudioRelationshipView: update trust/morale bars
   → ActiveProjectsPanel: add new project card
   → AudioManager: play stamp SFX
```

Key principle: **Simulation layer is pure logic with zero UI dependencies.** It returns state deltas, not visual updates. The controller translates deltas into UI events. This allows headless simulation testing, AI playthroughs, and potential multiplayer without touching simulation code.

### 2.3 Turn Structure & Phase Management

Each **Quarter** is a turn composed of sequential phases:

```
QUARTER START
  ↓
PHASE 1: GREENLIGHT TABLE (player-driven)
  - System generates 3-5 pitches from active studio pool
  - Player evaluates and decides: greenlight / pass / counter-offer
  - Repeat until player confirms "End Greenlight Phase"
  ↓
PHASE 2: MILESTONE REVIEWS (player-driven)
  - For each active project: advance 1 quarter of development
  - Check if milestone reached (alpha, beta, gold)
  - If milestone: present status report, offer intervention options
  - Player decides: intervene or restrain
  - Repeat for all active projects
  ↓
PHASE 3: ERA EVENTS (auto-resolve)
  - EraEngine checks current year/quarter
  - Triggers scheduled events (console launches, market shifts)
  - Applies 0-2 random events from event pool
  - Modifies simulation parameters (genre demand, platform install base, etc.)
  ↓
PHASE 4: RESULTS & SHIPPING (auto-resolve)
  - Projects reaching "gold" status ship this quarter
  - Quality_Seed + interventions + morale → final Metacritic score (with variance)
  - Revenue calculation based on quality, marketing, era demand
  - Portfolio matrix updates (Prestige/Profit/Populism)
  - Studio morale/trust updates based on results
  ↓
PHASE 5: QUARTERLY SUMMARY (player review)
  - Display: shipped games, scores, revenue, portfolio changes
  - Display: studio relationship updates
  - Player acknowledges → advances to next quarter
  ↓
IF QUARTER 4: ANNUAL REVIEW
  - Board evaluation (targets vs. actual)
  - Budget allocation for next year
  - Studio pool refresh (new studios enter based on reputation)
  - Player acknowledges → Year increments
  ↓
QUARTER END → loop to QUARTER START
```

Implementation: **Finite State Machine** in CampaignController with explicit phase transitions. Each phase has entry/exit hooks for UI setup/teardown. Phases block until player input or auto-resolve completes.

```csharp
public enum CampaignPhase {
    GreenlightTable,
    MilestoneReviews,
    EraEvents,
    ResultsAndShipping,
    QuarterlySummary,
    AnnualReview
}

public class CampaignController : MonoBehaviour {
    private CampaignPhase currentPhase;
    private CampaignState state;

    public void AdvancePhase() {
        ExitPhase(currentPhase);
        currentPhase = GetNextPhase(currentPhase);
        EnterPhase(currentPhase);
    }

    private void EnterPhase(CampaignPhase phase) {
        switch (phase) {
            case CampaignPhase.GreenlightTable:
                PitchGenerator.GeneratePitches(state);
                UIEventBus.Raise(new ShowGreenlightTableEvent());
                break;
            case CampaignPhase.MilestoneReviews:
                ProjectManager.AdvanceAllProjects(state);
                UIEventBus.Raise(new ShowMilestoneReviewEvent());
                break;
            // ... etc
        }
    }
}
```

Why FSM instead of coroutines or async? **Debuggability.** Phase state is explicit and visible in the inspector. Save/load can checkpoint at phase boundaries. Phase transitions are logged for playtest analysis.

---

## 3. Key Gameplay Systems — Technical Design

### 3.1 Pitch Generation System

**Design Goal**: Generate 3-5 unique, contextually relevant pitches each quarter from a pool of 50+ authored concepts without repetition or predictability.

**Architecture**:

```
ScriptableObject: PitchConcept
  - string conceptName
  - Genre genre
  - Platform[] targetPlatforms
  - int baseQualitySeed (50-85 range)
  - int baseBudget
  - int baseTimeline (in quarters)
  - StudioArchetype[] preferredArchetypes
  - string[] riskFactors
  - EraTag[] eraFitTags (e.g., "casual_boom", "next_gen_early")

ScriptableObject: StudioDefinition
  - string studioName
  - StudioArchetype archetype
  - int capability (0-100)
  - int startingMorale (0-100)
  - int startingReputation (0-100)
  - PitchConcept[] exclusiveConcepts (unlocked at Trust > 30)
```

**Pitch Generation Algorithm** (runs each Quarter during Greenlight phase):

```csharp
public List<Pitch> GeneratePitches(CampaignState state) {
    List<Studio> eligibleStudios = GetEligibleStudios(state);
    // Eligible = not currently working on a project, morale > 15,
    // trust > -30, hasn't pitched in last 2 quarters

    int pitchCount = Random.Range(3, 6);
    List<Pitch> pitches = new List<Pitch>();

    for (int i = 0; i < pitchCount && eligibleStudios.Count > 0; i++) {
        Studio studio = PickWeightedStudio(eligibleStudios, state);
        eligibleStudios.Remove(studio);

        PitchConcept concept = SelectConceptForStudio(studio, state);

        Pitch pitch = new Pitch {
            studio = studio,
            concept = concept,
            budget = CalculateBudget(concept, state.currentYear),
            timeline = CalculateTimeline(concept, studio.capability),
            qualitySeed = CalculateQualitySeed(concept, studio, state),
            marketFit = EraEngine.EvaluateMarketFit(concept, state),
            gutFeel = CalculateGutFeel(qualitySeed, state.executiveIntuition),
            riskFactors = DetermineRiskFactors(concept, studio, state)
        };

        pitches.Add(pitch);
    }

    return pitches;
}

private int CalculateQualitySeed(PitchConcept concept, Studio studio,
                                   CampaignState state) {
    float studioFactor = studio.capability * 0.35f;
    float conceptFit = EraEngine.GetConceptFitScore(concept, state) * 0.25f;
    float moraleFactor = studio.morale * 0.15f;
    float randomVariance = Random.Range(-12.5f, 12.5f) * 0.25f;

    return Mathf.Clamp((int)(studioFactor + conceptFit + moraleFactor +
                             randomVariance), 20, 95);
}

private int CalculateGutFeel(int qualitySeed, int intuitionLevel) {
    float noiseRange = 15f / Mathf.Pow(1.5f, intuitionLevel - 1);
    // Level 1: ±15, Level 2: ±10, Level 3: ±6.7, Level 4: ±4.4, Level 5: ±3

    int noise = Random.Range(-(int)noiseRange, (int)noiseRange);
    return Mathf.Clamp(qualitySeed + noise, 30, 95);
}
```

**Performance**: Pitch generation runs once per quarter. With 15 studios and 50 concepts, worst-case O(n*m) = 750 iterations. Negligible. Cache era fit scores per concept (recalculate only when era changes).

**Data Authoring**: Designers create PitchConcept and StudioDefinition ScriptableObjects in editor. No code changes to add new pitches or studios. Inspector preview shows calculated quality seed ranges and archetype fit.

### 3.2 Intervention System

**Design Goal**: 15+ interventions with studio-archetype-specific morale modifiers, budget/timeline impacts, and hidden quality seed changes.

**Architecture**:

```csharp
[CreateAssetMenu]
public class InterventionDefinition : ScriptableObject {
    public string interventionName;
    public int baseBudgetImpact; // millions
    public int baseTimelineImpact; // quarters
    public int baseMoraleImpact;
    public AnimationCurve qualitySeedModifier;
    // X-axis: project's current quality seed, Y-axis: delta

    public Dictionary<StudioArchetype, int> archetypeMoraleModifiers;
    // e.g., Auteur: -25, Reliable: -5 for "Force Multiplayer"

    public string[] prerequisiteFlags; // e.g., "has_multiplayer", "genre_action"
    public string[] grantedFlags; // applied after intervention
}

public class InterventionResult {
    public int budgetDelta;
    public int timelineDelta;
    public int moraleDelta;
    public int qualitySeedDelta; // hidden from player
    public string narrativeFeedback; // email snippet, studio reaction
}
```

**Intervention Execution**:

```csharp
public InterventionResult ApplyIntervention(InterventionDefinition intervention,
                                             ProjectState project,
                                             Studio studio) {
    InterventionResult result = new InterventionResult();

    // Budget and timeline are straightforward
    result.budgetDelta = intervention.baseBudgetImpact;
    result.timelineDelta = intervention.baseTimelineImpact;

    // Morale is base + archetype modifier
    int archetypeMod = intervention.archetypeMoraleModifiers
                                   .GetValueOrDefault(studio.archetype, 0);
    result.moraleDelta = intervention.baseMoraleImpact + archetypeMod;

    // Quality seed uses animation curve for non-linear scaling
    float currentQuality = project.qualitySeed;
    result.qualitySeedDelta = (int)intervention.qualitySeedModifier
                                               .Evaluate(currentQuality);

    // Apply to project and studio
    project.budget += result.budgetDelta * 1_000_000;
    project.remainingQuarters += result.timelineDelta;
    project.qualitySeed = Mathf.Clamp(project.qualitySeed +
                                      result.qualitySeedDelta, 20, 95);
    studio.morale = Mathf.Clamp(studio.morale + result.moraleDelta, 0, 100);

    // Narrative feedback lookup
    result.narrativeFeedback = GetNarrativeFeedback(intervention,
                                                    studio.archetype,
                                                    result.moraleDelta);

    return result;
}
```

**Data-Driven Design**: Each intervention is a ScriptableObject. Designers tweak values in inspector, hit Play, see results. Animation curves allow non-linear effects (e.g., "Delay to Polish" has diminishing returns: first delay +8 quality, second +5, third +2).

**Morale Death Spiral Prevention**: When studio morale drops below 30, quality seed penalty kicks in. The simulation flags this as a "crisis" state. UI should surface this clearly (red warning on milestone review). Intervention tooltips show predicted morale impact before applying.

### 3.3 Studio AI & Relationship System

**Design Goal**: 12-15 persistent studios with memory, personality, and evolving relationships over 10-year campaign.

**Studio State**:

```csharp
public class Studio {
    public string studioName;
    public StudioArchetype archetype;

    // Core stats
    public int capability; // 0-100, modifies quality seed
    public int morale; // 0-100, affects output and pitch willingness
    public int trust; // -50 to +50, accumulated relationship score
    public int reputation; // 0-100, industry standing (affects prestige)
    public int desperation; // 0-100, financial pressure

    // State tracking
    public int quartersSinceLastPitch;
    public int consecutiveHits; // streak of 80+ Metacritic games
    public int consecutiveFlops; // streak of <60 Metacritic games
    public bool hasPitchedMagnumOpus;
    public List<int> shippedProjectIDs; // history

    // Decay modifiers (for Fading archetype)
    public int capabilityDecayPerYear; // typically -5 for Fading studios
}
```

**Trust Mechanics**:

Trust accumulates based on player actions. Every interaction modifies trust:

```csharp
// Trust modification examples
Greenlight at full budget: +5
Greenlight at reduced budget: -3 to -8 (scales with cut severity)
Pass on pitch: -3
Cancel project mid-development: -20 to -40 (scales with timeline % complete)
Ship a hit (80+ Metacritic): +8
Ship a flop (<60 Metacritic): -5
Intervene (any): -1 to -3 depending on archetype sensitivity
Restraint (no intervention): +3 (Auteurs: +8)

// Inter-studio communication
When you cancel ANY project:
  For each studio where trust < +10: trust -= 3

When you fire a lead designer (intervention):
  All Auteur studios: trust -= 8 regardless of relationship
```

**Archetype Behavior**:

Archetypes are implemented as strategy pattern behaviors:

```csharp
public interface IStudioBehavior {
    float GetPitchFrequencyModifier(Studio studio, CampaignState state);
    int GetQualitySeedVariance(Studio studio);
    int GetInterventionMoralePenalty(InterventionDefinition intervention);
    PitchConcept SelectPreferredConcept(Studio studio,
                                         List<PitchConcept> availableConcepts);
    bool ShouldOfferMagnumOpus(Studio studio, CampaignState state);
}

public class AuteurBehavior : IStudioBehavior {
    public bool ShouldOfferMagnumOpus(Studio studio, CampaignState state) {
        return studio.trust > 30 &&
               state.currentYear >= 2007 &&
               !studio.hasPitchedMagnumOpus;
    }

    public int GetInterventionMoralePenalty(InterventionDefinition intervention) {
        return intervention.baseMoraleImpact * 2; // double penalty for Auteurs
    }
}

public class FadingBehavior : IStudioBehavior {
    public void OnYearEnd(Studio studio, CampaignState state) {
        studio.capability = Mathf.Max(30,
                                      studio.capability +
                                      studio.capabilityDecayPerYear);

        // Check for reinvention condition
        var recentOriginalIP = GetRecentShippedOriginalIP(studio, state);
        if (recentOriginalIP != null && recentOriginalIP.metacritic >= 80) {
            studio.capabilityDecayPerYear = 3; // reverse decay
        }
    }
}
```

Behaviors are assigned at studio initialization. Archetype-specific logic is isolated in behavior classes, not scattered through simulation code.

**Inter-Studio Reputation Events**:

Studios talk. Major player actions trigger reputation cascades:

```csharp
public void OnProjectCancelled(ProjectState project, Studio studio) {
    // Direct impact on studio
    studio.morale -= 40;
    studio.trust -= 30;
    studio.desperation += 20;

    // Reputation cascade: other studios notice
    foreach (Studio otherStudio in allStudios) {
        if (otherStudio.trust < 10) {
            otherStudio.trust -= 3;
            LogReputationEvent($"{otherStudio.studioName} is wary after " +
                               $"{studio.studioName} cancellation");
        }
    }

    // Portfolio prestige hit
    portfolioManager.ModifyPrestige(-10);
}
```

These cascades create systemic consequences. Cancel one project and you damage relationships with 6 other studios. This is emergent — the player discovers through experimentation that cruelty has costs.

### 3.4 Era Engine

**Design Goal**: Historical industry events (2003-2012) that modify simulation parameters and create strategic opportunities/threats.

**Architecture**:

```csharp
[CreateAssetMenu]
public class EraEvent : ScriptableObject {
    public int triggerYear;
    public int triggerQuarter; // 1-4, or 0 for "any quarter"
    public string eventName;
    public string eventDescription; // shown in news feed UI

    public GenreModifier[] genreModifiers;
    // e.g., "casual games: +20 demand, +15% revenue multiplier"

    public PlatformModifier[] platformModifiers;
    // e.g., "Wii: install base growth rate 3x"

    public GlobalModifier[] globalModifiers;
    // e.g., "all next-gen budgets +30%"

    public bool requiresPlayerResponse; // e.g., board demands casual strategy
}

public class EraEngine {
    private List<EraEvent> scheduledEvents;
    private Dictionary<Genre, float> currentGenreDemand;
    private Dictionary<Platform, float> currentPlatformInstallBase;

    public void ProcessQuarter(CampaignState state) {
        // Check for scheduled events
        var triggeredEvents = scheduledEvents.Where(e =>
            e.triggerYear == state.currentYear &&
            (e.triggerQuarter == 0 || e.triggerQuarter == state.currentQuarter));

        foreach (var eraEvent in triggeredEvents) {
            ApplyEraEvent(eraEvent, state);
            UIEventBus.Raise(new EraEventTriggeredEvent(eraEvent));
        }

        // Random event roll (2-3 per year)
        if (Random.value < 0.25f) {
            EraEvent randomEvent = PickRandomEvent(state);
            ApplyEraEvent(randomEvent, state);
        }
    }

    private void ApplyEraEvent(EraEvent eraEvent, CampaignState state) {
        foreach (var mod in eraEvent.genreModifiers) {
            currentGenreDemand[mod.genre] += mod.demandDelta;
        }

        foreach (var mod in eraEvent.platformModifiers) {
            currentPlatformInstallBase[mod.platform] *= mod.growthMultiplier;
        }

        // Global modifiers affect all future calculations
        state.ApplyGlobalModifiers(eraEvent.globalModifiers);
    }

    public int EvaluateMarketFit(PitchConcept concept, CampaignState state) {
        float genreFit = currentGenreDemand.GetValueOrDefault(concept.genre, 50f);
        float platformFit = currentPlatformInstallBase
                              .GetValueOrDefault(concept.targetPlatforms[0], 50f);
        float eraTagFit = CalculateEraTagFit(concept.eraFitTags, state);

        return Mathf.RoundToInt((genreFit * 0.4f) +
                                (platformFit * 0.3f) +
                                (eraTagFit * 0.3f));
    }
}
```

**Event Data**: Era events are ScriptableObjects authored in editor. Example:

```
EraEvent: "Wii Launch"
Trigger: Year 2006, Quarter 4
Genre Modifiers:
  - Casual: demand +25, revenue multiplier +0.20
  - Party: demand +30, revenue multiplier +0.25
  - Core Action: demand -10 (audience fragmentation)
Platform Modifiers:
  - Wii: install base growth 3.5x baseline
  - GameCube: end of life, growth 0x
Global Modifiers:
  - Motion control games: quality seed +5 if "casual" genre
Requires Response: Yes (board demands casual strategy within 2 quarters)
```

**Rumor System** (1-2 quarter advance warning):

Major era events spawn "rumor" events 1-2 quarters before they trigger:

```csharp
public class EraRumor {
    public EraEvent upcomingEvent;
    public int quartersUntilTrigger;
    public string rumorText; // "Sources say Nintendo is developing..."
}
```

Rumors appear in news feed UI. Players who read and position accordingly (greenlight Wii games in Q2 2006 for Q4 2006 ship) get strategic advantage.

**Performance**: Era engine runs once per quarter. Event lookup is O(log n) via sorted list. Modifier application is O(genres + platforms) ~= O(20). Negligible cost. Genre/platform demand values are cached; only recalculate on event trigger.

---

## 4. Portfolio & Economy Systems

### 4.1 Portfolio Matrix

**Three-axis scoring system**: Prestige, Profit, Populism (each 0-100).

```csharp
public class PortfolioManager {
    public int prestige;
    public int profit;
    public int populism;

    public List<ShippedGame> shippedGames;

    public void OnGameShipped(ProjectState project, int metacritic,
                              int revenue, int unitsSold) {
        ShippedGame game = new ShippedGame {
            projectName = project.concept.conceptName,
            studio = project.studio,
            metacritic = metacritic,
            revenue = revenue,
            unitsSold = unitsSold,
            yearShipped = campaignState.currentYear
        };

        shippedGames.Add(game);

        // Calculate axis deltas
        int prestigeDelta = CalculatePrestigeDelta(game, project);
        int profitDelta = CalculateProfitDelta(game, project);
        int populismDelta = CalculatePopulismDelta(game, project);

        prestige = Mathf.Clamp(prestige + prestigeDelta, 0, 100);
        profit = Mathf.Clamp(profit + profitDelta, 0, 100);
        populism = Mathf.Clamp(populism + populismDelta, 0, 100);

        LogPortfolioChange(game, prestigeDelta, profitDelta, populismDelta);
    }

    private int CalculatePrestigeDelta(ShippedGame game, ProjectState project) {
        float metacriticFactor = (game.metacritic - 70) * 0.4f;
        float studioPrestige = project.studio.reputation * 0.3f;
        float innovationBonus = project.concept.isNewIP ? 10f : 0f;

        return Mathf.RoundToInt(metacriticFactor +
                                (studioPrestige / 100f * 15f) +
                                innovationBonus);
    }

    private int CalculateProfitDelta(ShippedGame game, ProjectState project) {
        float roi = (float)(game.revenue - project.totalBudget) /
                     project.totalBudget;
        float profitContribution = roi * 15f;

        if (project.concept.isFranchiseEntry) {
            profitContribution += 5f; // franchise bonus
        }

        return Mathf.RoundToInt(profitContribution);
    }

    private int CalculatePopulismDelta(ShippedGame game, ProjectState project) {
        float unitsFactor = (float)game.unitsSold / 500000f * 10f;
        float platformReach = CalculatePlatformReachMultiplier(project) * 5f;
        float marketingBonus = project.marketingSpend > 10_000_000 ? 3f : 0f;

        return Mathf.RoundToInt(unitsFactor + platformReach + marketingBonus);
    }
}
```

**Visualization**: Portfolio matrix is rendered as a triangle radar chart using Unity UI Toolkit Canvas API. Chart updates with smooth lerp animations on value changes. Each axis has a target threshold line (set by board); current value pulses green if above target, red if below.

### 4.2 Revenue & Budget Economy

**Revenue Calculation** (executed when game ships):

```csharp
public class RevenueCalculator {
    public int CalculateUnits(ProjectState project, int metacritic,
                              CampaignState state) {
        // Base formula from design doc
        int qualityContribution = metacritic * 8000;
        int marketingContribution = (project.marketingSpend / 1_000_000) * 3000;
        int franchiseBonus = project.concept.isFranchiseEntry ? 50000 : 0;
        int eraDemandBonus = EraEngine.GetDemandBonus(project.concept, state);

        // Franchise fatigue (negative feedback loop)
        if (project.concept.franchiseInstallment > 1) {
            int fatiguePercent = (project.concept.franchiseInstallment - 1) * 5;
            qualityContribution -= qualityContribution * fatiguePercent / 100;
        }

        int totalUnits = qualityContribution + marketingContribution +
                         franchiseBonus + eraDemandBonus;

        // Random variance ±10%
        totalUnits += Random.Range(-totalUnits / 10, totalUnits / 10);

        return Mathf.Max(50000, totalUnits); // floor at 50K
    }

    public int CalculateRevenue(int units, ProjectState project,
                                CampaignState state) {
        int pricePerUnit = GetPricePerUnit(project.concept.platform,
                                           state.currentYear);
        int baseRevenue = units * pricePerUnit;

        // DLC revenue (if mandated during development)
        if (project.hasDLCPlan) {
            float dlcMultiplier = CalculateDLCMultiplier(project.concept.genre);
            baseRevenue += (int)(baseRevenue * dlcMultiplier);
        }

        return baseRevenue;
    }

    private int GetPricePerUnit(Platform platform, int year) {
        if (platform == Platform.Console) {
            return year >= 2008 ? 60 : 50; // $50 -> $60 in 2008
        } else if (platform == Platform.Handheld) {
            return 30;
        } else if (platform == Platform.Digital) {
            return 15;
        }
        return 50;
    }
}
```

**Budget Tracking**:

Campaign starts with annual budget (e.g., Year 1: $80M). Each greenlight deducts from pool. Interventions further deduct. Board adjusts budget annually based on performance.

```csharp
public class BudgetManager {
    public int annualBudget;
    public int remainingBudget;
    public List<BudgetAllocation> allocations;

    public bool CanAfford(int cost) {
        return remainingBudget >= cost;
    }

    public void AllocateBudget(int cost, string description) {
        if (!CanAfford(cost)) {
            throw new InsufficientBudgetException();
        }

        remainingBudget -= cost;
        allocations.Add(new BudgetAllocation {
            amount = cost,
            description = description,
            quarter = campaignState.currentQuarter,
            year = campaignState.currentYear
        });

        UIEventBus.Raise(new BudgetChangedEvent(remainingBudget));
    }

    public void OnYearEnd(int boardSatisfaction) {
        // Board adjusts budget based on performance
        if (boardSatisfaction > 110) {
            annualBudget = (int)(annualBudget * 1.10f);
        } else if (boardSatisfaction < 70) {
            annualBudget = (int)(annualBudget * 0.90f);
        }

        remainingBudget = annualBudget;
    }
}
```

**Performance**: All economy calculations are integer math (revenue in dollars, not floats). No floating-point drift over 40 quarters. Budget state is trivially serializable.

---

## 5. AI/Simulation Systems

### 5.1 Quality Seed to Metacritic Resolution

The Hidden Quality Seed is calculated during pitch generation and modified by interventions throughout development. When the game ships, it resolves to a final Metacritic score with variance.

```csharp
public int ResolveMetacriticScore(ProjectState project) {
    int qualitySeed = project.qualitySeed;

    // Morale penalty (death spiral mechanic)
    if (project.studio.morale < 30) {
        int moralePenalty = (30 - project.studio.morale) / 2;
        qualitySeed -= moralePenalty;
    }

    // Intervention quality modifiers (accumulated during development)
    qualitySeed += project.accumulatedQualityModifiers;

    // Final variance ±5 points (simulates review subjectivity)
    int variance = Random.Range(-5, 6);

    int metacritic = Mathf.Clamp(qualitySeed + variance, 35, 98);

    return metacritic;
}
```

Why ±5 final variance? Two reasons:
1. **Replayability**: Same decisions shouldn't always produce identical scores
2. **Verisimilitude**: Real Metacritic scores have subjective variance

The variance is small enough that player decisions dominate outcomes but large enough that a 79 vs. 81 outcome creates tension.

### 5.2 Board AI

The Board is not a player-facing AI agent but an evaluation function that judges portfolio performance.

```csharp
public class BoardEvaluator {
    public BoardTargets currentTargets;

    public int CalculateSatisfaction(PortfolioManager portfolio) {
        float prestigeScore = Mathf.Min(
            (float)portfolio.prestige / currentTargets.prestigeMin, 1.5f) * 100f;
        float profitScore = Mathf.Min(
            (float)portfolio.profit / currentTargets.profitMin, 1.5f) * 100f;
        float populismScore = Mathf.Min(
            (float)portfolio.populism / currentTargets.populismMin, 1.5f) * 100f;

        // Weighted average (Profit weighted highest)
        float satisfaction = (prestigeScore * 0.30f) +
                             (profitScore * 0.45f) +
                             (populismScore * 0.25f);

        return Mathf.RoundToInt(satisfaction);
    }

    public void OnYearEnd(CampaignState state, PortfolioManager portfolio) {
        int satisfaction = CalculateSatisfaction(portfolio);

        if (satisfaction > 110) {
            // Reward
            state.budgetManager.annualBudget =
                (int)(state.budgetManager.annualBudget * 1.10f);
            state.maxActiveProjects += 1;
            LogBoardFeedback("Exceptional performance. Budget increased.");
        } else if (satisfaction < 70) {
            // Warning/penalty
            state.boardWarnings++;
            if (state.boardWarnings >= 2) {
                state.budgetManager.annualBudget =
                    (int)(state.budgetManager.annualBudget * 0.75f);
                LogBoardFeedback("Unacceptable. Budget cut 25%.");
            }
        } else if (satisfaction < 50 && state.boardWarnings >= 1) {
            // Termination
            TriggerGameOver(state, portfolio);
        }

        // Escalate targets each year
        EscalateTargets(state.currentYear);
    }
}
```

Board behavior is deterministic and transparent. Players should be able to predict board reactions based on portfolio state. No hidden AI tricks.

### 5.3 Studio Pitch Willingness

Studios decide whether to pitch each quarter based on morale, trust, and cooldown.

```csharp
public bool WillStudioPitch(Studio studio, CampaignState state) {
    // Hard gates
    if (studio.morale < 15) return false; // too demoralized
    if (studio.trust < -30) return false; // relationship destroyed
    if (studio.quartersSinceLastPitch < 2) return false; // cooldown
    if (studio.currentlyWorkingOnProject) return false; // busy

    // Probabilistic factors
    float pitchChance = 0.7f; // base 70% chance

    // Trust modifies willingness
    pitchChance += studio.trust * 0.005f; // +25% at trust +50, -25% at -50

    // Desperation increases pitch frequency
    pitchChance += studio.desperation * 0.003f; // +30% at max desperation

    // Archetype-specific modifiers
    pitchChance *= studio.behavior.GetPitchFrequencyModifier(studio, state);

    return Random.value < pitchChance;
}
```

This creates emergent studio behavior. Cancel a project → trust drops → studio stops pitching. Multiple failures → desperation rises → studio pitches desperately even with low trust. Auteur studios with high trust pitch magnum opus concepts.

---

## 6. Save System Design

### 6.1 Requirements

- **Checkpoint save**: Auto-save at the end of each quarter
- **Manual save**: Player can save at any time during decision phases
- **Multiple save slots**: Minimum 3, ideally 10
- **Cloud save**: Steam Cloud integration for PC
- **Corruption resilience**: Detect corrupt saves, fall back to last valid auto-save
- **Save migration**: Support version updates without breaking old saves

### 6.2 Save Data Structure

Entire campaign state serializes to JSON. Unity's JsonUtility is sufficient (no need for external libraries).

```csharp
[Serializable]
public class CampaignSaveData {
    public int saveVersion; // for migration
    public DateTime saveTimestamp;

    public int currentYear;
    public int currentQuarter;
    public CampaignPhase currentPhase;

    public BudgetData budget;
    public PortfolioData portfolio;
    public StudioData[] studios;
    public ProjectData[] activeProjects;
    public ShippedGameData[] shippedGames;

    public EraEngineData eraState;
    public BoardData boardState;

    public int executiveIntuitionLevel;
    public int correctPredictions;

    public string saveGameName; // player-defined
}

[Serializable]
public class StudioData {
    public string studioID;
    public int capability;
    public int morale;
    public int trust;
    public int reputation;
    public int desperation;
    public int quartersSinceLastPitch;
    public bool hasPitchedMagnumOpus;
    public int[] shippedProjectIDs;
}

[Serializable]
public class ProjectData {
    public int projectID;
    public string studioID;
    public string conceptID;
    public int budget;
    public int totalBudget; // including intervention costs
    public int remainingQuarters;
    public int qualitySeed;
    public int accumulatedQualityModifiers;
    public int marketingSpend;
    public bool hasDLCPlan;
    public string[] interventionHistory;
}
```

### 6.3 Save/Load Implementation

```csharp
public class SaveManager {
    private const string SAVE_PATH = "/Saves/";
    private const int CURRENT_SAVE_VERSION = 1;

    public void SaveGame(CampaignState state, string saveName) {
        CampaignSaveData saveData = SerializeCampaignState(state);
        saveData.saveGameName = saveName;
        saveData.saveVersion = CURRENT_SAVE_VERSION;
        saveData.saveTimestamp = DateTime.Now;

        string json = JsonUtility.ToJson(saveData, true);
        string filePath = Application.persistentDataPath + SAVE_PATH +
                          saveName + ".json";

        // Write to temp file first, then atomic rename (prevents corruption)
        string tempPath = filePath + ".tmp";
        File.WriteAllText(tempPath, json);

        if (File.Exists(filePath)) {
            File.Delete(filePath);
        }
        File.Move(tempPath, filePath);

        Debug.Log($"Saved: {filePath}");
    }

    public CampaignState LoadGame(string saveName) {
        string filePath = Application.persistentDataPath + SAVE_PATH +
                          saveName + ".json";

        if (!File.Exists(filePath)) {
            throw new FileNotFoundException($"Save not found: {saveName}");
        }

        string json = File.ReadAllText(filePath);
        CampaignSaveData saveData = JsonUtility.FromJson<CampaignSaveData>(json);

        // Version migration
        if (saveData.saveVersion < CURRENT_SAVE_VERSION) {
            saveData = MigrateSave(saveData, CURRENT_SAVE_VERSION);
        }

        CampaignState state = DeserializeCampaignState(saveData);
        return state;
    }

    public void AutoSave(CampaignState state) {
        SaveGame(state, "autosave_" + state.currentYear + "_Q" +
                        state.currentQuarter);

        // Keep only last 5 auto-saves
        PruneOldAutoSaves();
    }
}
```

**Auto-Save Triggers**:
- End of each quarter (after quarterly summary phase)
- Before major phase transitions (before annual review)
- After greenlight decisions (in case of crash during milestone review)

**Corruption Detection**:

```csharp
public bool ValidateSaveData(CampaignSaveData saveData) {
    // Basic sanity checks
    if (saveData.currentYear < 2003 || saveData.currentYear > 2012)
        return false;
    if (saveData.budget.remainingBudget < 0)
        return false;
    if (saveData.studios == null || saveData.studios.Length == 0)
        return false;

    // Referential integrity
    foreach (var project in saveData.activeProjects) {
        if (!saveData.studios.Any(s => s.studioID == project.studioID)) {
            Debug.LogError($"Orphaned project: {project.projectID}");
            return false;
        }
    }

    return true;
}
```

If validation fails, fall back to last known good auto-save and notify player.

### 6.4 Save Migration Strategy

When we ship updates that change save data structure:

```csharp
private CampaignSaveData MigrateSave(CampaignSaveData oldData,
                                     int targetVersion) {
    CampaignSaveData newData = oldData;

    for (int v = oldData.saveVersion; v < targetVersion; v++) {
        switch (v) {
            case 1:
                // Hypothetical: v1 -> v2 adds DLC system
                foreach (var project in newData.activeProjects) {
                    if (!project.hasDLCPlan) {
                        project.hasDLCPlan = false; // default value
                    }
                }
                break;

            case 2:
                // Hypothetical: v2 -> v3 adds new studio archetype
                // Migration logic here
                break;
        }
    }

    newData.saveVersion = targetVersion;
    return newData;
}
```

Never break old saves. Migrate gracefully with default values for new fields.

---

## 7. Performance Budget & Optimization Strategy

### 7.1 Target Performance

| Platform | Target | Justification |
|---|---|---|
| PC (mid-range) | 60 FPS UI, instant (<100ms) simulation | Turn-based game; no excuse for frame drops |
| Nintendo Switch | 30 FPS UI, <200ms simulation | Handheld mode worst-case; UI complexity is moderate |
| Mobile tablet | 30 FPS UI, <300ms simulation | Touch input latency tolerance higher |

### 7.2 Performance Budget Breakdown

**Per-Frame Budget** (16.6ms at 60 FPS):

- UI rendering: 8ms
- Input handling: 1ms
- Audio: 2ms
- Simulation update (if any): 0ms (simulation is turn-based, not frame-based)
- Buffer: 5.6ms

**Per-Turn Budget** (player clicks "End Quarter"):

- Pitch generation: 10ms
- Project milestone advancement: 5ms per project × 6 max = 30ms
- Era event processing: 5ms
- Revenue/portfolio calculations: 10ms
- Save auto-save: 50ms (async, doesn't block UI)
- **Total: ~100ms worst-case**

### 7.3 Hot Paths & Optimization Points

**Hot Path #1: Pitch Generation** (runs every quarter)

Current algorithm is O(studios × concepts) = O(15 × 50) = 750 iterations worst-case. With caching of era fit scores, this is ~10ms on target hardware. No optimization needed unless playtesting shows issues.

**Optimization if needed**:
- Pre-compute studio-concept compatibility matrix at campaign start
- Cache quality seed calculations for concept-studio pairs
- Parallelize pitch generation (Unity Job System)

**Hot Path #2: UI Updates After Greenlight**

Player greenlights a pitch → 5 UI panels update (greenlight table, budget, active projects, studio relationships, portfolio projection). Currently implemented as event-driven updates. Each panel subscribes to relevant events and updates only changed data.

**Anti-pattern to avoid**: Rebuilding entire UI hierarchies on every change. Use data binding (UI Toolkit's binding system) to update only changed fields.

```csharp
// GOOD: Data binding updates only changed value
budgetLabel.binding = new DataBinding {
    dataSourcePath = "remainingBudget",
    bindingMode = BindingMode.ToTarget
};

// BAD: Manual full rebuild
void OnBudgetChanged() {
    RebuildEntireBudgetPanel(); // destroys and recreates all UI elements
}
```

**Hot Path #3: Save System**

JSON serialization of full campaign state: ~500KB typical, 2MB worst-case (Year 10 with all history). JsonUtility serialization: ~30ms. File write: ~20ms. **Total: 50ms.** Acceptable for auto-save.

**Optimization if needed**: Async save on background thread (C# Thread or Unity Job). UI shows "Saving..." indicator, doesn't block input.

### 7.4 Memory Budget

| Component | Estimated Size | Notes |
|---|---|---|
| UI textures/atlases | 50 MB | UI-only game, minimal assets |
| ScriptableObject definitions | 5 MB | Studio/pitch/intervention/era data |
| Campaign state (runtime) | 2 MB | Full 10-year state at max size |
| Audio assets | 30 MB | SFX + ambient + music stems |
| **Total** | **~90 MB** | Trivial for all platforms |

No streaming or asset management required. Everything loads at boot. Campaign state lives in memory for entire session.

### 7.5 Profiling Strategy

Don't optimize until profiling shows problems. But profile early and often.

**Profiling schedule**:
- **Week 4**: Profile pitch generation with 50 concepts, 15 studios
- **Week 6**: Profile full quarter turn with 6 active projects
- **Week 8**: Profile UI responsiveness under rapid input (spam-clicking pitches)
- **Week 10**: Profile save/load with worst-case Year 10 save data
- **Week 12**: Full campaign playthrough with Unity Profiler running; identify any frame spikes or GC stalls

**Profiling tools**:
- Unity Profiler (CPU, memory, rendering)
- Deep Profiling for simulation layer (identify O(n²) accidents)
- Memory Profiler for GC allocation hotspots

**Red flags to watch for**:
- Any per-frame GC allocations in UI code (use object pooling for dynamic lists)
- O(n²) loops in pitch generation or intervention processing
- Blocking file I/O on main thread during save

---

## 8. Platform Considerations

### 8.1 PC (Steam) — Primary Platform

**Input**: Mouse + keyboard. UI optimized for mouse hover states, tooltips, precise clicking.

**Resolution**: Support 1920×1080 through 3840×2160 (4K). UI scales via Unity Canvas Scaler (scale with screen size mode).

**Steam Integration**:
- **Steam Cloud**: Auto-upload save files. Max 100MB per user (our saves are ~2MB max, plenty of headroom).
- **Achievements**: 30-40 achievements tied to campaign milestones, portfolio outcomes, studio relationships.
- **Stats tracking**: Total games greenlit, average Metacritic, studios championed, etc. Surfaced in Steam stats API.

**Build Pipeline**: Unity Cloud Build or local Windows build. Target x64 architecture only (x86 is dead).

### 8.2 Nintendo Switch — Secondary Platform

**Input**: Joy-Con or Pro Controller. UI must support button navigation (no mouse cursor).

**Controls Mapping**:
- A button: Confirm / Select pitch
- B button: Cancel / Pass
- D-pad: Navigate pitches/panels
- Shoulder buttons (L/R): Quick-switch between UI tabs
- Plus: Menu / Save
- Minus: Back / Undo

**UI Adaptation**:
- Increase button sizes 20% (thumb vs. mouse precision)
- Add button prompt icons (Switch-style A/B/X/Y glyphs)
- Selected UI elements highlight with thick border
- Tooltips auto-expand on focus (can't "hover" with controller)

**Performance**:
- Target 30 FPS (acceptable for turn-based UI game)
- Test in handheld mode (worst-case: 720p undocked)
- Reduce UI particle effects if performance is borderline

**Build Pipeline**: Unity's official Switch SDK (requires Nintendo developer approval). Build for ARMv8 architecture.

**Platform-Specific Code**:
```csharp
#if UNITY_SWITCH
    // Use Switch-specific save path
    string savePath = nn.fs.SaveData.GetPath();
#else
    string savePath = Application.persistentDataPath;
#endif
```

### 8.3 Mobile (Tablet) — Stretch Platform

**Input**: Touch. UI must support tap, swipe, pinch-to-zoom (for pitch dossiers).

**Adaptation**:
- Touch targets minimum 44pt (Apple HIG guideline)
- Greenlight stamp becomes a tap-and-hold gesture (0.5s hold) for intentionality
- Swipe left/right to navigate pitches
- Pinch to zoom on portfolio matrix and complex charts

**Performance**:
- Target 30 FPS on iPad Air (A12 chip) and equivalent Android tablets
- Reduce UI shader complexity (no real-time blur effects)

**Monetization** (if we go mobile):
- Premium upfront price ($9.99) — no F2P, no IAP
- Or: First 2 years free, unlock full campaign for $4.99

**Platform-Specific Considerations**:
- iOS requires portrait + landscape support (prefer landscape for UI density)
- Android fragmentation: test on 3-4 common tablet resolutions

### 8.4 Cross-Platform Save Sync

**PC ↔ Switch**: No built-in sync (Steam Cloud vs. Nintendo Cloud are separate). Accept this limitation for v1. Potential v2 feature: custom cloud backend.

**PC ↔ Mobile**: Possible via custom backend (Firebase, PlayFab). Not a priority unless mobile platform proves viable.

---

## 9. Build Pipeline & Tools

### 9.1 Unity Build Settings

**Scripting Backend**:
- PC: Mono (faster build iteration)
- Switch: IL2CPP (required by platform)
- Mobile: IL2CPP (better performance, smaller binary)

**Code Stripping**: Medium (remove unused Unity modules, keep reflection for JSON serialization)

**Compression**: LZ4 (fast decompression, acceptable size for ~90MB asset footprint)

### 9.2 Version Control

**Git** with **Git LFS** for binary assets (ScriptableObject meta files, audio, UI sprites).

**Branch Strategy**:
- `main`: Stable, releasable builds
- `develop`: Active development, daily commits
- `feature/*`: Individual features (e.g., `feature/era-engine`, `feature/intervention-system`)

**Commit Discipline**:
- Commit compiled ScriptableObjects, not just scripts (designers need to pull playable data)
- .gitignore: `Library/`, `Temp/`, `Builds/`, `*.csproj`, `*.sln`

### 9.3 Automated Testing

**Unit Tests** (Unity Test Framework):

Focus on simulation layer. UI is hard to test; simulation is pure logic and must be bulletproof.

```csharp
[Test]
public void GreenlightDeductsBudget() {
    CampaignState state = new CampaignState { remainingBudget = 80_000_000 };
    Pitch pitch = new Pitch { budget = 25_000_000 };

    SimulationLayer.ExecuteGreenlight(pitch, state);

    Assert.AreEqual(55_000_000, state.remainingBudget);
}

[Test]
public void InterventionModifiesQualitySeed() {
    ProjectState project = new ProjectState { qualitySeed = 70 };
    InterventionDefinition intervention =
        ScriptableObject.CreateInstance<InterventionDefinition>();
    intervention.qualitySeedModifier = AnimationCurve.Constant(0, 100, 5);

    SimulationLayer.ApplyIntervention(intervention, project);

    Assert.AreEqual(75, project.qualitySeed);
}

[Test]
public void MoraleDeathSpiralPenalizesQuality() {
    ProjectState project = new ProjectState { qualitySeed = 80 };
    Studio studio = new Studio { morale = 10 };
    project.studio = studio;

    int metacritic = RevenueCalculator.ResolveMetacriticScore(project);

    Assert.Less(metacritic, 70); // morale penalty should drop score significantly
}
```

**Target**: 80% code coverage on simulation layer. Run tests pre-commit via Git hook.

**Integration Tests**:

Simulate full quarters and validate state consistency:

```csharp
[Test]
public void FullQuarterSimulation() {
    CampaignState state = SetupCampaignYear2005();

    // Generate pitches
    List<Pitch> pitches = PitchGenerator.GeneratePitches(state);
    Assert.IsTrue(pitches.Count >= 3 && pitches.Count <= 5);

    // Greenlight first pitch
    SimulationLayer.ExecuteGreenlight(pitches[0], state);

    // Advance quarter
    CampaignController.AdvanceQuarter(state);

    // Validate state
    Assert.AreEqual(2, state.currentQuarter);
    Assert.AreEqual(1, state.activeProjects.Count);
}
```

### 9.4 Designer Tools

**ScriptableObject Inspector Extensions**:

Custom editors for complex data types:

```csharp
[CustomEditor(typeof(InterventionDefinition))]
public class InterventionEditor : Editor {
    public override void OnInspectorGUI() {
        DrawDefaultInspector();

        InterventionDefinition intervention = (InterventionDefinition)target;

        // Preview quality seed curve
        EditorGUILayout.LabelField("Quality Seed Modifier Preview:");
        for (int q = 50; q <= 90; q += 10) {
            float delta = intervention.qualitySeedModifier.Evaluate(q);
            EditorGUILayout.LabelField($"  Quality {q}: {delta:+0;-0;0}");
        }

        // Archetype morale preview
        EditorGUILayout.LabelField("Morale Impact by Archetype:");
        foreach (StudioArchetype archetype in System.Enum.GetValues(
                                                typeof(StudioArchetype))) {
            int morale = intervention.archetypeMoraleModifiers
                                     .GetValueOrDefault(archetype, 0);
            EditorGUILayout.LabelField($"  {archetype}: {morale:+0;-0;0}");
        }
    }
}
```

**Pitch Simulator Tool** (custom editor window):

Designers can simulate pitch outcomes without playing the game:

```csharp
public class PitchSimulatorWindow : EditorWindow {
    private StudioDefinition testStudio;
    private PitchConcept testConcept;
    private int testYear = 2005;

    [MenuItem("Tools/Pitch Simulator")]
    public static void ShowWindow() {
        GetWindow<PitchSimulatorWindow>("Pitch Simulator");
    }

    void OnGUI() {
        testStudio = (StudioDefinition)EditorGUILayout.ObjectField(
            "Studio", testStudio, typeof(StudioDefinition), false);
        testConcept = (PitchConcept)EditorGUILayout.ObjectField(
            "Concept", testConcept, typeof(PitchConcept), false);
        testYear = EditorGUILayout.IntSlider("Year", testYear, 2003, 2012);

        if (GUILayout.Button("Simulate Pitch")) {
            CampaignState mockState = CreateMockCampaignState(testYear);
            Pitch result = PitchGenerator.GeneratePitch(testStudio,
                                                        testConcept,
                                                        mockState);

            EditorGUILayout.LabelField($"Quality Seed: {result.qualitySeed}");
            EditorGUILayout.LabelField($"Gut Feel: {result.gutFeel}");
            EditorGUILayout.LabelField($"Market Fit: {result.marketFit}");
            EditorGUILayout.LabelField($"Budget: ${result.budget:N0}");
        }
    }
}
```

**Campaign Timeline Visualizer**:

Tool to visualize full campaign arc (which studios pitched when, which games shipped, portfolio trajectory):

```csharp
public class CampaignTimelineWindow : EditorWindow {
    private CampaignSaveData loadedSave;

    void OnGUI() {
        if (GUILayout.Button("Load Save File")) {
            string path = EditorUtility.OpenFilePanel("Load Campaign Save",
                                                      "", "json");
            loadedSave = LoadSaveData(path);
        }

        if (loadedSave != null) {
            DrawTimeline(loadedSave);
        }
    }

    private void DrawTimeline(CampaignSaveData save) {
        // Visualize 10-year timeline with shipped games, studio relationships,
        // portfolio trajectory as line graphs
        // Useful for post-mortem analysis and balancing
    }
}
```

---

## 10. Technical Risk Assessment

### Risk Matrix

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| **Save corruption** | Medium | Critical | Atomic file writes, validation checks, auto-save rotation |
| **UI performance degradation** | Low | High | Profile early, data binding instead of full rebuilds, object pooling |
| **Simulation complexity explosion** | Medium | High | Strict data-driven design, unit tests, designer tools for validation |
| **Studio AI feels random/unfair** | Medium | Medium | Transparent formulas, tooltips showing calculations, playtesting |
| **Switch port performance issues** | Low | Medium | Profile on devkit early (Week 8), reduce UI effects if needed |
| **Metacritic resolution feels arbitrary** | Medium | High | Clear feedback on quality seed contributors, ±5 variance is tight enough |
| **Balance death spiral** | High | Medium | Playtesting with spreadsheet models, tunable difficulty knobs |
| **Mid-game pacing sag** | High | Medium | Playtest drop-off tracking, scripted mid-game events if needed |
| **Cross-platform input inconsistency** | Low | Low | Abstract input layer, test all platforms by Week 10 |

### High-Priority Risks & Detailed Mitigation

**Risk: Save Corruption**

A corrupted save in a 15-hour campaign is catastrophic. Players will uninstall.

**Mitigation**:
1. Atomic file writes (write to .tmp, rename on success)
2. JSON schema validation before deserializing
3. Referential integrity checks (no orphaned projects, all studio IDs valid)
4. Rotation of 5 most recent auto-saves (if latest is corrupt, fall back)
5. Manual save slots separate from auto-saves (player-initiated backups)
6. QA test: Simulate power loss mid-save (kill process during File.Write)

**Risk: Simulation Complexity Explosion**

As we add interventions, era events, and studio behaviors, the interaction space grows quadratically. Untested edge cases will cause bugs.

**Mitigation**:
1. Data-driven design: Interventions/events are ScriptableObjects, not hard-coded
2. Unit tests for every intervention × archetype combination
3. Designer tools to preview outcomes before runtime
4. Simulation logs: Every decision logged with inputs/outputs for post-mortem
5. Playtest with "chaos mode" that spams random interventions to find edge cases

**Risk: Balance Death Spiral**

If player makes 2-3 bad decisions early, they could enter an unwinnable state (no budget, low trust across all studios, board on verge of firing).

**Mitigation**:
1. Board warnings give 2-quarter grace period before consequences
2. Era events can provide "lifelines" (e.g., recession lowers board expectations)
3. Studio desperation mechanic means even low-trust studios will pitch eventually
4. Playtest with intentionally bad decisions to validate recovery paths
5. Difficulty knobs (hidden config file) for tuning board tolerance, budget recovery

**Risk: Studio AI Feels Random**

If players can't predict studio reactions, interventions feel like slot machines.

**Mitigation**:
1. Tooltips show exact morale impact before applying intervention
2. Studio archetype descriptions explain sensitivities ("Auteurs hate interference")
3. Relationship screen shows trust/morale trends over time (graphs, not just numbers)
4. Tutorial explicitly teaches archetype differences via guided examples
5. Post-ship studio emails reference specific player decisions ("Thanks for the autonomy")

---

## 11. Development Milestones (Tech Perspective)

### Phase 1: Core Loop Prototype (Weeks 1-4)

**Goal**: Prove the 30-second greenlight decision loop is fun.

**Deliverables**:
- CampaignState, Studio, Pitch, and Project data structures
- Pitch generation algorithm (5 studios, 10 concepts, hard-coded)
- Greenlight decision execution (budget deduction, project creation)
- Basic UI: Greenlight Table with 3 pitch cards, stamp animation
- Save/load (JSON serialization of minimal state)

**Tech Validation**:
- Can generate 3-5 pitches in <10ms?
- Does quality seed calculation feel opaque but fair?
- Is UI responsive to rapid decisions?

**Playtest Focus**: Does stamping the greenlight feel weighty? Do players hesitate?

---

### Phase 2: Intervention System (Weeks 5-7)

**Goal**: Implement the 5-minute loop (milestone reviews + interventions).

**Deliverables**:
- ProjectManager: Advance quarters, track milestones
- Intervention system: 8 core interventions as ScriptableObjects
- Studio morale/trust modification logic
- UI: Milestone review screen, intervention menu with tooltips
- Quality seed hidden modifiers

**Tech Validation**:
- Do interventions modify project state correctly?
- Are archetype-specific morale penalties working?
- Does restraint feel like a deliberate choice?

**Playtest Focus**: Do players understand intervention tradeoffs? Do they intervene reactively or strategically?

---

### Phase 3: Portfolio & Board (Weeks 8-10)

**Goal**: Implement meta loop (annual reviews, board targets).

**Deliverables**:
- PortfolioManager: Prestige/Profit/Populism tracking
- BoardEvaluator: Annual reviews, target escalation
- RevenueCalculator: Metacritic resolution, unit sales, revenue
- UI: Portfolio radar chart, board meeting scene, annual summary

**Tech Validation**:
- Are portfolio axis calculations accurate?
- Does board satisfaction formula match design doc?
- Do players understand why axes moved?

**Playtest Focus**: Can players predict portfolio impacts? Does the board feel fair or arbitrary?

---

### Phase 4: Era Engine (Weeks 11-13)

**Goal**: Implement historical industry events and market shifts.

**Deliverables**:
- EraEngine: Event scheduling, genre/platform demand modifiers
- 20 core era events (console launches, market disruptions)
- Rumor system (1-2 quarter warnings)
- UI: News feed, era event notifications

**Tech Validation**:
- Do era events correctly modify simulation parameters?
- Is market fit calculation accurate?
- Are rumors telegraphed clearly?

**Playtest Focus**: Do players anticipate era shifts? Do they feel clever when a bet pays off?

---

### Phase 5: Studio AI & Relationships (Weeks 14-16)

**Goal**: Implement persistent studio personalities and long-term arcs.

**Deliverables**:
- Studio archetype behaviors (Auteur, Reliable, Volatile, Fading)
- Trust accumulation system
- Inter-studio reputation cascades
- Magnum opus pitch unlock (Auteur, Trust > 30)
- UI: Studio relationship panel, trust/morale graphs

**Tech Validation**:
- Do studios behave according to archetype?
- Does trust accumulation feel meaningful over multiple years?
- Do reputation cascades (e.g., after cancellation) propagate correctly?

**Playtest Focus**: Do players form opinions about studios? Do they remember studio names?

---

### Phase 6: Full Campaign Integration (Weeks 17-20)

**Goal**: Wire all systems into a complete 10-year campaign.

**Deliverables**:
- Full 2003-2012 timeline with all era events
- 50+ authored pitch concepts
- 12-15 studio definitions
- Opening scenario (inherited cancellation)
- Endgame: Legacy montage, termination epilogue
- Save/load: Full state serialization, auto-save rotation

**Tech Validation**:
- Can complete a full 10-year campaign without crashes?
- Are save files stable across sessions?
- Is performance acceptable at Year 10 (max complexity)?

**Playtest Focus**: Full campaign pacing. Where do players drop off? Is the ending satisfying?

---

### Phase 7: Polish & Platform Ports (Weeks 21-24)

**Goal**: Ship-ready builds for PC, Switch.

**Deliverables**:
- PC build: Steam integration (Cloud saves, achievements)
- Switch build: Controller support, performance optimization
- Audio: Full SFX pass, music implementation
- UI polish: Animations, transitions, feedback improvements
- QA: Full platform testing, edge case hunting

**Tech Validation**:
- Does Switch build hit 30 FPS in handheld mode?
- Are Steam Cloud saves syncing correctly?
- Are there any save corruption edge cases?

**Playtest Focus**: Final balance pass. Are board targets too harsh? Too lenient?

---

### Phase 8: Launch Prep (Weeks 25-26)

**Goal**: Launch-ready, stable, marketed.

**Deliverables**:
- Day-one patch pipeline
- Launch trailer (capture footage from campaign)
- Steam page live
- Press builds
- Post-launch support plan (bug triage, balance patches)

---

## 12. Post-Launch Technical Roadmap

### Patch 1.1 (Month 2)

- Bug fixes from launch feedback
- Balance tuning (board targets, intervention costs)
- QoL: Fast-forward quarter button, undo last decision

### Patch 1.2 (Month 4)

- New studio archetypes (2-3 additional)
- New interventions (5 additional)
- Expanded random event pool

### Expansion: "The Indie Boom" (Month 8)

- Extend campaign to 2015 (add 3 years, 12 quarters)
- Digital distribution dominance era
- Crowdfunding mechanics
- Early Access greenlighting

### Potential Sequel Scope

- Multiplayer competitive mode (2 publishers bidding on studios)
- Rival publisher AI
- Franchise system (multi-installment planning)
- Custom campaign editor (players author studios/pitches)

---

## 13. Appendix: Technology Stack Summary

| Component | Technology | Rationale |
|---|---|---|
| **Engine** | Unity 2022 LTS | Cross-platform, mature UI, fast iteration |
| **Language** | C# | Unity standard, strong typing, good tooling |
| **UI Framework** | UI Toolkit | Data-bindable, performant, scalable |
| **Serialization** | JSON (Unity JsonUtility) | Human-readable, debuggable, version-migratable |
| **Version Control** | Git + Git LFS | Industry standard, handles binary assets |
| **Testing** | Unity Test Framework | Built-in, supports unit/integration tests |
| **Profiling** | Unity Profiler | Deep insights into CPU, memory, rendering |
| **CI/CD** | Unity Cloud Build (optional) | Automated builds for multiple platforms |
| **Analytics** | Unity Analytics (opt-in) | Player behavior tracking for balance tuning |

---

## Closing Notes

This is a data-driven, systems-heavy management sim. The technical architecture prioritizes three things:

1. **Iteration speed** — Designers tweak ScriptableObjects, hit Play, see results immediately
2. **State integrity** — Save/load is bulletproof; 15-hour campaigns must never corrupt
3. **System clarity** — Simulation layer is pure logic with zero UI coupling; testable, debuggable, understandable

The complexity is in the simulation, not the rendering. The challenge is managing state interactions (12 studios × 50 pitches × 15 interventions × 40 quarters = millions of possible states). The solution is aggressive data-driven design, unit testing, and designer tooling.

Ship the simplest version that proves the core loop. Profile before optimizing. Trust the data, not your gut — unless your gut has leveled up to Intuition 5.

The greenlight stamp is the game. Everything else is in service of making that stamp feel like it matters.

---

**Document Status**: Production-ready. Approved for implementation.

**Next Steps**: Week 1 kickoff. Prototype the Greenlight Table. 30-second loop or bust.

— BYTE
