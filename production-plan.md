# GREENLIGHT — Production Plan

**Producer**: CLOCK
**Version**: 1.0
**Date**: 2026-02-07
**Status**: Production-Ready

---

> "Cut scope, not corners. A shipped game beats a perfect concept."
> — CLOCK

---

## 1. Project Overview

### What We're Shipping

GREENLIGHT is a publisher management simulation where players greenlight, fund, and shepherd game projects through a decade of industry upheaval (2003-2012). The player never makes games — they decide which games get made, then live with the consequences.

**Core Experience**: 30-second pitch evaluation loops nested inside 5-minute milestone management phases, wrapped in a 12-18 hour campaign spanning 40 quarters of decision-making. Every greenlight is a bet. Every bet has a legacy.

**Primary Platform**: PC (Steam)
**Secondary Platform**: Nintendo Switch
**Stretch Platform**: Mobile tablets

**Target Ship Date**: Q4 2026 (26 weeks from kickoff)
**Team Size**: 6-8 developers (1 producer, 1 designer, 1 narrative, 1 programmer, 1 UI/UX artist, 1 audio designer, 1 QA lead, +1 contractor for portraits)

### Scope Reality Check

This is a data-driven simulation wrapped in UI. No real-time gameplay. No physics. No combat. No procedural generation beyond pitch selection. The complexity is in systems interaction, not technical wizardry. That's the good news.

The bad news: We're simulating 12-15 AI studio entities with persistent memory across 10 years, 50+ authored game concepts, 15+ interventions with archetype-specific responses, a three-axis portfolio system, historical industry events, and a relationship web where every decision ripples. The state space is massive. We're not building an engine. We're building an economy.

**What keeps me up at night**: Mid-game pacing sag (Year 5-7), save corruption, and the morale death spiral becoming unwinnable instead of tense. The greenlight stamp must feel satisfying after 200 uses, not annoying.

---

## 2. Milestone Definitions

### Milestone 1: Core Loop Prototype (Weeks 1-4)

**Entry Criteria**:
- Unity 2022 LTS project initialized
- Git repo configured with LFS
- Team onboarded, roles assigned

**Exit Criteria (Vertical Slice Quality)**:
- Greenlight Table UI functional (3-5 pitch cards, dossier display)
- Pitch generation algorithm working (5 studios, 10 concepts, quality seed calculation)
- Greenlight stamp animation implemented and satisfying
- Budget deduction and project creation functional
- Basic save/load (JSON serialization of minimal state)
- Playtest-ready build (1 fiscal year playable, ~15 minutes)

**Deliverables**:
- CampaignState, Studio, Pitch, Project data structures (C#)
- ScriptableObject templates for studios and pitches
- UI Toolkit implementation of Greenlight Table
- Stamp SFX placeholder (ECHO provides temp asset)

**Success Metric**: Does the 30-second loop feel weighty? Do playtesters hesitate before stamping? If 70%+ of testers report the stamp feels satisfying, we proceed. If not, we iterate on feedback timing, sound, and animation weight.

**Risk**: If pitch evaluation feels like homework instead of poker, the game is dead. This milestone proves or kills the core thesis.

**Team Allocation**:
- BYTE: Simulation layer (pitch generation, quality seed, greenlight execution)
- PIXEL: Greenlight Table UI, stamp animation
- ECHO: Stamp SFX (3 variants), UI feedback sounds
- REED: Pitch concept authoring (10 concepts as ScriptableObjects)
- CLOCK: Playtest coordination, milestone tracking

---

### Milestone 2: Intervention System (Weeks 5-7)

**Entry Criteria**:
- M1 approved (core loop validated by playtest)
- Pitch generation stable and performant

**Exit Criteria**:
- 8 core interventions implemented as ScriptableObjects
- Milestone review UI functional (project status display, intervention menu)
- Studio morale/trust modification logic working
- Quality seed hidden modifiers apply correctly
- Archetype-specific morale penalties functional
- Restraint option feels deliberate (not a non-choice)

**Deliverables**:
- InterventionDefinition ScriptableObjects (Force Multiplayer, Ship It Now, Delay to Polish, Cut Features, Expand Scope, Cancel Project, Increase Marketing, Mandate DLC)
- Milestone review screen (UI Toolkit)
- Intervention tooltips showing predicted impacts
- ProjectManager: quarterly advancement, milestone tracking
- Studio morale/trust bar animations

**Success Metric**: Do playtesters understand intervention tradeoffs? Can they predict morale impact within ±5 points? Do 60%+ of testers choose restraint at least once (proving it's a real choice)?

**Risk**: If interventions feel arbitrary or tooltips are unclear, players will avoid the system entirely and miss half the game's depth.

**Team Allocation**:
- BYTE: Intervention execution logic, morale/trust systems
- REED: Intervention design and balancing (archetype modifiers)
- PIXEL: Milestone review UI, intervention button design
- ECHO: Morale shift SFX (glass fill/drain, crisis alerts)

---

### Milestone 3: Portfolio & Board (Weeks 8-10)

**Entry Criteria**:
- M2 approved (intervention system validated)
- Revenue calculation spec finalized

**Exit Criteria**:
- Portfolio Matrix (Prestige/Profit/Populism) functional
- Board evaluation logic working (satisfaction calculation, target escalation)
- Metacritic resolution implemented (quality seed to final score)
- Revenue calculation accurate (units sold, DLC, ROI)
- Annual review UI complete (board meeting, portfolio radar chart)
- First shipped game can complete full loop (greenlight to Metacritic reveal)

**Deliverables**:
- PortfolioManager: axis tracking, delta calculations
- BoardEvaluator: satisfaction formula, annual targets, warnings/termination
- RevenueCalculator: units, pricing, DLC, franchise fatigue
- Portfolio triangle radar chart (UI Toolkit Canvas API)
- Board evaluation screen (board member portraits, feedback text)

**Success Metric**: Can playtesters predict why portfolio axes moved after shipping a game? Do they understand what the board wants? If 70%+ can articulate the Prestige/Profit/Populism tension, we're clear.

**Risk**: If the portfolio matrix feels opaque or the board seems unfair, players will feel punished without understanding why. Transparency is non-negotiable.

**Team Allocation**:
- BYTE: Portfolio calculations, board logic, revenue formulas
- REED: Board target tuning, axis weight balancing
- PIXEL: Portfolio radar chart, board meeting scene UI
- ECHO: Board ambience, Metacritic reveal SFX
- NOVA: Board member character writing (feedback text)

---

### Milestone 4: Era Engine (Weeks 11-13)

**Entry Criteria**:
- M3 approved (meta loop validated)
- Historical era event timeline authored (20 core events)

**Exit Criteria**:
- Era Engine functional (event scheduling, genre/platform modifiers)
- 20 core era events implemented (console launches, market disruptions)
- Rumor system working (1-2 quarter advance warnings)
- Market fit calculation accurate (era demand cross-referenced with pitch concepts)
- News feed UI displays era events clearly
- Player can anticipate and position for era shifts

**Deliverables**:
- EraEvent ScriptableObjects (Wii launch, Xbox 360, DLC emergence, recession, etc.)
- EraEngine: event triggering, demand modifiers, rumor generation
- News feed UI (era notification display)
- Genre/platform demand tracking (dictionaries updated per event)

**Success Metric**: Do playtesters anticipate era shifts 60-70% of the time (via rumors)? Do they cite era positioning as part of their strategy in post-session interviews?

**Risk**: If era events feel like external randomness instead of strategic opportunities, the 10-year arc collapses into repetitive quarters. Rumors must telegraph clearly.

**Team Allocation**:
- BYTE: Era Engine implementation, event triggering logic
- REED: Era event authoring, demand modifier balancing
- NOVA: Era event narrative text (news headlines, descriptions)
- PIXEL: News feed UI, era notification design

---

### Milestone 5: Studio AI & Relationships (Weeks 14-16)

**Entry Criteria**:
- M4 approved (era system validated)
- Studio archetype behaviors spec finalized

**Exit Criteria**:
- Studio archetype behaviors implemented (Auteur, Reliable, Volatile, Fading)
- Trust accumulation system working across multiple years
- Inter-studio reputation cascades functional (e.g., cancel project affects all studios)
- Magnum opus pitch unlocks correctly (Auteur, Trust > 30, Year 7+)
- Studio relationship panel shows trust/morale trends over time
- Studios feel like persistent characters, not random pitch generators

**Deliverables**:
- IStudioBehavior interface + 4 archetype implementations
- Trust modification on all player actions (greenlight, pass, cancel, intervene)
- Reputation cascade logic (OnProjectCancelled, OnLeadDesignerFired)
- Studio relationship UI panel (trust/morale graphs, archetype badges)
- Magnum opus pitch unlock logic

**Success Metric**: Do playtesters remember studio names unprompted after 2-hour session? Do 60%+ cite a specific studio relationship in post-test feedback? If players talk about studios like characters, we've succeeded.

**Risk**: If studios feel interchangeable, the long-game relationship investment collapses. Archetypes must be behaviorally distinct, not just flavor text.

**Team Allocation**:
- BYTE: Studio behavior system, trust/reputation cascades
- REED: Archetype tuning, pitch selection logic
- NOVA: Studio personality writing (email voices, pitch presentation styles)
- PIXEL: Studio relationship panel UI, trust/morale visualization

---

### Milestone 6: Full Campaign Integration (Weeks 17-20)

**Entry Criteria**:
- M5 approved (studio system validated)
- All 50+ pitch concepts authored
- All 12-15 studio definitions authored
- Opening scenario scripted (inherited cancellation)

**Exit Criteria**:
- Full 2003-2012 timeline playable (40 quarters, 10 years)
- Opening scenario functional (inherited cancellation decision)
- Endgame sequences complete (Legacy Montage, Termination Epilogue)
- Save/load stable across full campaign (auto-save rotation, corruption detection)
- Performance acceptable at Year 10 (max complexity)
- 12-18 hour full playthrough possible without crashes

**Deliverables**:
- All 50 PitchConcept ScriptableObjects
- All 12-15 StudioDefinition ScriptableObjects
- Opening scenario UI + narrative scripting
- Legacy Montage sequence (timeline visualization, final scores)
- Termination Epilogue sequence
- Save manager: auto-save rotation, validation, migration framework

**Success Metric**: Can QA complete a full 10-year campaign without encountering blocking bugs? Are save files stable across sessions? Is the ending emotionally resonant (60%+ of testers report feeling "reflective" or "impacted")?

**Risk**: This is where pacing sag reveals itself. If playtesters drop off at Year 5-7, we need mid-game narrative events or pacing adjustments. If saves corrupt, we delay ship.

**Team Allocation**:
- REED + NOVA: Pitch/studio authoring (parallel work on content)
- BYTE: Save system hardening, full campaign state management
- PIXEL: Opening/endgame sequence UI
- ECHO: Legacy Montage music (full orchestral piece)
- CRASH: Full campaign QA pass, edge case hunting

---

### Milestone 7: Polish & Platform Ports (Weeks 21-24)

**Entry Criteria**:
- M6 approved (full campaign playable and stable)
- Steam integration spec finalized
- Switch devkit accessible

**Exit Criteria**:
- PC build: Steam Cloud saves working, achievements implemented
- Switch build: Controller support functional, 30 FPS in handheld mode
- Audio: Full SFX pass complete, music stems implemented in FMOD
- UI polish: All animations tuned, transitions smooth, feedback clear
- Accessibility: Keyboard navigation, screen reader labels, colorblind modes
- QA: Platform-specific testing complete, no critical bugs

**Deliverables**:
- PC build with Steam integration (Cloud saves, achievements, stats)
- Switch build with controller mapping, performance optimization
- Complete audio implementation (200+ SFX, music stems, adaptive system)
- UI animation polish pass (stamp weight, morale bar timing, portfolio triangle morphs)
- Accessibility features (keyboard nav, high contrast mode, visual redundancy)

**Success Metric**: Does Switch build hit 30 FPS consistently in handheld mode? Are Steam Cloud saves syncing correctly across devices? Do accessibility features pass WCAG 2.1 Level AA automated tests?

**Risk**: Switch performance issues could delay ship. Profile early (Week 8 on devkit) to catch problems before crunch.

**Team Allocation**:
- BYTE: Steam integration, Switch port optimization
- PIXEL: UI animation polish, accessibility implementation
- ECHO: Full audio implementation (SFX integration, FMOD mixing)
- CRASH: Platform-specific QA, edge case validation

---

### Milestone 8: Launch Prep (Weeks 25-26)

**Entry Criteria**:
- M7 approved (platform builds stable)
- Steam page live
- Press build ready

**Exit Criteria**:
- Day-one patch pipeline established
- Launch trailer captured and edited
- Steam page finalized (screenshots, capsule art, description)
- Press builds distributed
- Post-launch support plan documented

**Deliverables**:
- Launch trailer (90 seconds, three-act structure per art direction doc)
- Steam page assets (capsule images, screenshots, key art)
- Day-one patch build (bug fixes from final QA pass)
- Post-launch roadmap (Patch 1.1 scope defined)

**Success Metric**: Are we ready to ship on target date? Is the trailer compelling enough to convert wishlists?

**Team Allocation**:
- HYPE: Trailer editing, Steam page copy
- PIXEL: Marketing asset creation (capsule images, key art)
- BYTE: Day-one patch bug fixes
- CRASH: Final QA validation
- CLOCK: Launch coordination, press outreach

---

## 3. Sprint Plan (26-Week Breakdown)

### Sprint Structure

2-week sprints. 13 sprints total. Each sprint ends with a review and retrospective.

**Sprint cadence**:
- Monday: Sprint planning (2 hours)
- Daily: 15-minute standups
- Friday EOD: Sprint review (1 hour) + retrospective (30 minutes)
- Saturday/Sunday: No work (protect the team)

### Sprint 1-2: Core Loop Prototype (M1)

**Sprint 1 (Weeks 1-2)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| Unity project setup, Git repo config | BYTE | 1 | None | P0 |
| CampaignState, Studio, Pitch data structures | BYTE | 2 | None | P0 |
| Pitch generation algorithm (hard-coded 10 concepts) | BYTE | 3 | Data structures | P0 |
| Greenlight Table UI layout (UI Toolkit) | PIXEL | 3 | None | P0 |
| Pitch dossier card design (mockup + implementation) | PIXEL | 2 | Greenlight Table | P0 |
| Stamp SFX recording (3 variants) | ECHO | 1 | None | P0 |
| Budget ticker UI | PIXEL | 1 | Greenlight Table | P1 |
| Basic save/load (JSON serialization) | BYTE | 2 | Data structures | P1 |

**Sprint 2 (Weeks 3-4)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| Greenlight stamp animation (1.2s, mechanical weight) | PIXEL | 2 | Dossier card | P0 |
| Quality seed calculation logic | BYTE | 2 | Pitch generation | P0 |
| Gut-feel indicator (noisy signal from quality seed) | BYTE | 2 | Quality seed | P0 |
| ScriptableObject templates (PitchConcept, StudioDefinition) | REED + BYTE | 2 | Data structures | P0 |
| 10 pitch concepts authored | REED | 3 | ScriptableObject templates | P0 |
| 5 studio definitions authored | REED | 2 | ScriptableObject templates | P0 |
| Stamp SFX integration + budget ticker sound | ECHO | 1 | Stamp animation | P0 |
| Playtest build preparation | BYTE | 1 | All systems | P0 |
| **M1 Playtest Session** | CLOCK | 0.5 | Build ready | P0 |

**Critical Path**: Pitch generation → Quality seed → Greenlight execution → Stamp animation → Playtest

**Risk**: If stamp doesn't feel satisfying by end of Sprint 2, we iterate before proceeding. Non-negotiable.

---

### Sprint 3-4: Intervention System (M2)

**Sprint 3 (Weeks 5-6)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| ProjectManager: quarterly advancement logic | BYTE | 2 | CampaignState | P0 |
| Milestone tracking (alpha, beta, gold phases) | BYTE | 2 | ProjectManager | P0 |
| InterventionDefinition ScriptableObject template | BYTE | 1 | None | P0 |
| 8 core interventions authored (design + data) | REED | 3 | InterventionDefinition | P0 |
| Milestone review UI layout | PIXEL | 3 | None | P0 |
| Intervention menu UI (pill buttons, hover tooltips) | PIXEL | 2 | Milestone review | P0 |
| Morale shift SFX (glass fill/drain) | ECHO | 1 | None | P1 |

**Sprint 4 (Weeks 7)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| Intervention execution logic (morale, budget, timeline impacts) | BYTE | 3 | InterventionDefinition | P0 |
| Studio morale/trust modification system | BYTE | 2 | Studio data structures | P0 |
| Archetype-specific morale penalties | BYTE | 2 | Interventions | P0 |
| Restraint option UI + feedback | PIXEL | 1 | Intervention menu | P0 |
| Morale bar animation (fill/drain with color shift) | PIXEL | 2 | Milestone review | P0 |
| Crisis alert SFX | ECHO | 1 | Morale system | P1 |
| **M2 Playtest Session** | CLOCK | 0.5 | All systems | P0 |

**Critical Path**: InterventionDefinition → Execution logic → Morale system → Archetype penalties → Playtest

**Risk**: If intervention tooltips don't clearly communicate tradeoffs, iterate on UI presentation before proceeding.

---

### Sprint 5-6: Portfolio & Board (M3)

**Sprint 5 (Weeks 8-9)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| PortfolioManager: axis tracking (Prestige/Profit/Populism) | BYTE | 2 | None | P0 |
| Portfolio delta calculations (per shipped game) | BYTE | 3 | PortfolioManager | P0 |
| RevenueCalculator: units sold formula | BYTE | 2 | None | P0 |
| RevenueCalculator: pricing, DLC, franchise fatigue | BYTE | 2 | Units sold | P0 |
| BoardEvaluator: satisfaction calculation | BYTE | 2 | PortfolioManager | P0 |
| Portfolio radar chart UI (triangle visualization) | PIXEL | 3 | PortfolioManager | P0 |
| Board meeting scene UI layout | PIXEL | 2 | None | P1 |

**Sprint 6 (Weeks 10)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| Metacritic resolution (quality seed to final score) | BYTE | 2 | Quality seed | P0 |
| Board target escalation per year | BYTE | 1 | BoardEvaluator | P0 |
| Board warning/termination logic | BYTE | 2 | BoardEvaluator | P0 |
| Board member portrait integration (temp assets) | PIXEL | 1 | Board scene | P1 |
| Board feedback text authoring | NOVA | 2 | Board logic | P0 |
| Metacritic reveal SFX + score-dependent tails | ECHO | 2 | Metacritic resolution | P0 |
| **M3 Playtest Session** | CLOCK | 0.5 | All systems | P0 |

**Critical Path**: Portfolio calculations → Revenue formulas → Board satisfaction → Metacritic resolution → Playtest

**Risk**: If portfolio axis movement feels arbitrary, add more detailed feedback (which games contributed what). Transparency over mystery.

---

### Sprint 7-8: Era Engine (M4)

**Sprint 7 (Weeks 11-12)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| EraEvent ScriptableObject template | BYTE | 1 | None | P0 |
| Era Engine: event scheduling system | BYTE | 2 | EraEvent | P0 |
| Genre/platform demand modifier system | BYTE | 2 | Era Engine | P0 |
| 10 core era events authored (Xbox 360, Wii, DLC, recession, etc.) | REED | 3 | EraEvent template | P0 |
| Era event narrative text (news headlines) | NOVA | 2 | Era events | P0 |
| News feed UI layout | PIXEL | 2 | None | P0 |

**Sprint 8 (Weeks 13)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| Remaining 10 era events authored | REED | 2 | Sprint 7 | P0 |
| Rumor system (1-2 quarter advance warnings) | BYTE | 2 | Era Engine | P0 |
| Market fit calculation (era demand cross-reference) | BYTE | 2 | Genre/platform modifiers | P0 |
| Era event SFX (positive, neutral, disruptive stings) | ECHO | 2 | Era events | P0 |
| News feed transition animations | PIXEL | 1 | News feed UI | P1 |
| **M4 Playtest Session** | CLOCK | 0.5 | All systems | P0 |

**Critical Path**: Era Engine → Event authoring → Rumor system → Market fit → Playtest

**Risk**: If rumors don't telegraph clearly enough, players will feel blindsided. Iterate on UI presentation and timing.

---

### Sprint 9-10: Studio AI & Relationships (M5)

**Sprint 9 (Weeks 14-15)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| IStudioBehavior interface design | BYTE | 1 | None | P0 |
| Auteur behavior implementation | BYTE | 2 | IStudioBehavior | P0 |
| Reliable behavior implementation | BYTE | 1 | IStudioBehavior | P0 |
| Volatile behavior implementation | BYTE | 2 | IStudioBehavior | P0 |
| Fading behavior implementation | BYTE | 2 | IStudioBehavior | P0 |
| Trust accumulation system | BYTE | 2 | Studio data structures | P0 |
| Studio relationship panel UI | PIXEL | 3 | None | P0 |

**Sprint 10 (Weeks 16)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| Reputation cascade logic (OnProjectCancelled, etc.) | BYTE | 2 | Trust system | P0 |
| Magnum opus pitch unlock (Auteur, Trust > 30) | BYTE | 2 | Auteur behavior | P0 |
| Studio email voice writing (4 archetypes × 3 moods) | NOVA | 3 | Archetypes | P0 |
| Trust/morale graph visualization | PIXEL | 2 | Relationship panel | P0 |
| Studio departure SFX (burned bridge) | ECHO | 1 | Reputation cascades | P1 |
| **M5 Playtest Session** | CLOCK | 0.5 | All systems | P0 |

**Critical Path**: Archetype behaviors → Trust system → Reputation cascades → Relationship UI → Playtest

**Risk**: If studios feel interchangeable, the long game collapses. Archetypes must be behaviorally distinct and memorable.

---

### Sprint 11-12: Full Campaign Integration (M6)

**Sprint 11 (Weeks 17-18)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| Pitch concept authoring (50 total concepts) | REED | 5 | PitchConcept template | P0 |
| Studio definition authoring (12-15 total studios) | REED | 3 | StudioDefinition template | P0 |
| Opening scenario scripting (inherited cancellation) | NOVA | 2 | None | P0 |
| Opening scenario UI implementation | PIXEL | 2 | Scenario script | P0 |
| Save manager: auto-save rotation | BYTE | 2 | Save/load | P0 |
| Save validation and corruption detection | BYTE | 2 | Save manager | P0 |

**Sprint 12 (Weeks 19-20)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| Legacy Montage sequence (timeline visualization) | PIXEL | 3 | None | P0 |
| Legacy scoring system (Patron/Mogul/Architect) | BYTE | 2 | Portfolio + Studio data | P0 |
| Termination Epilogue sequence | PIXEL | 2 | None | P0 |
| Legacy Montage music composition | ECHO | 4 | None | P0 |
| Full campaign QA pass (40 quarters playthrough) | CRASH | 5 | All systems | P0 |
| Performance profiling at Year 10 | BYTE | 1 | Full campaign | P0 |
| Save migration framework | BYTE | 2 | Save manager | P1 |
| **M6 Full Campaign Playtest (12-hour session)** | CLOCK + Team | 1 | All systems | P0 |

**Critical Path**: Content authoring → Opening/endgame sequences → Full QA pass → Playtest

**Risk**: This is where pacing sag reveals itself. If playtest shows drop-off at Year 5-7, we need mid-game events or pacing adjustments before proceeding.

---

### Sprint 13-14: Polish & Platform Ports (M7)

**Sprint 13 (Weeks 21-22)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| Steam integration (Cloud saves) | BYTE | 2 | None | P0 |
| Steam achievements implementation (30 achievements) | BYTE | 2 | Steam integration | P0 |
| Switch controller support | BYTE | 3 | None | P0 |
| Switch performance optimization | BYTE | 3 | Controller support | P0 |
| Full SFX pass (200+ sounds) | ECHO | 5 | All systems | P0 |
| Music stem implementation in FMOD | ECHO | 3 | Music composition | P0 |

**Sprint 14 (Weeks 23-24)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| UI animation polish (stamp weight, transitions) | PIXEL | 3 | All UI systems | P0 |
| Accessibility: Keyboard navigation | PIXEL | 2 | UI polish | P0 |
| Accessibility: Screen reader labels | PIXEL | 1 | Keyboard nav | P0 |
| Accessibility: High contrast mode | PIXEL | 1 | UI polish | P1 |
| FMOD adaptive music system integration | ECHO | 3 | Music stems | P0 |
| Switch QA pass (handheld + docked modes) | CRASH | 4 | Switch build | P0 |
| PC QA pass (Steam features validation) | CRASH | 3 | PC build | P0 |
| **M7 Platform Validation** | CLOCK + CRASH | 1 | All platforms | P0 |

**Critical Path**: Steam integration → Switch port → Audio implementation → Accessibility → QA passes → Validation

**Risk**: Switch performance issues. If we don't hit 30 FPS in handheld mode by Week 22, we delay ship or cut platform. Profile early on devkit (Week 8) to avoid late surprises.

---

### Sprint 15: Launch Prep (M8)

**Sprint 15 (Weeks 25-26)**:

| Task | Owner | Estimate (days) | Dependency | Priority |
|---|---|---|---|---|
| Launch trailer capture (gameplay footage) | HYPE | 2 | Final build | P0 |
| Launch trailer editing (90 seconds) | HYPE | 2 | Footage | P0 |
| Steam page finalization (screenshots, capsule art) | PIXEL + HYPE | 2 | Final build | P0 |
| Day-one patch build (bug fixes from final QA) | BYTE | 3 | QA reports | P0 |
| Press build distribution | CLOCK | 1 | Final build | P0 |
| Post-launch roadmap documentation (Patch 1.1 scope) | CLOCK + REED | 1 | None | P1 |
| **Ship Date Readiness Review** | CLOCK + Team | 0.5 | All deliverables | P0 |

**Critical Path**: Trailer → Steam page → Day-one patch → Ship

**Success Criteria**: We ship on target date (Q4 2026, Week 26) with no critical bugs, Steam Cloud saves working, and a trailer that converts wishlists.

---

## 4. Risk Register

### Critical Risks (Likelihood: High, Impact: Critical)

**Risk #1: Mid-Game Pacing Sag (Years 5-7)**

- **Description**: Players drop off between Year 5-7 because the loop becomes repetitive without new mechanical hooks or narrative events.
- **Likelihood**: High (this is common in long-form management sims)
- **Impact**: Critical (kills completion rate, damages reviews)
- **Mitigation**:
  - Track playtest drop-off points during M6 full campaign playtest
  - If >30% drop-off in Years 5-7, add mid-game narrative events (e.g., studio mutiny, hostile takeover attempt, Eleanor Voss email)
  - Ensure Era Engine introduces disruptive events in Year 6-7 (DLC economy, recession) to refresh strategic space
- **Contingency**: If mitigation fails, compress campaign to 8 years (2003-2010) without losing core events
- **Owner**: CLOCK + REED

**Risk #2: Save Corruption**

- **Description**: 12-18 hour campaigns mean save corruption is catastrophic. Players will uninstall.
- **Likelihood**: Medium (JSON serialization generally stable, but edge cases exist)
- **Impact**: Critical (destroys player trust, review bomb risk)
- **Mitigation**:
  - Atomic file writes (write to .tmp, rename on success)
  - Save validation on load (referential integrity, sanity checks)
  - Auto-save rotation (keep 5 most recent, fall back if latest is corrupt)
  - QA test: Simulate power loss mid-save (kill process during File.Write)
- **Contingency**: If corruption discovered post-launch, emergency patch with save recovery tool
- **Owner**: BYTE

**Risk #3: Greenlight Stamp Loses Satisfaction After 200+ Uses**

- **Description**: The 30-second loop repeats 100+ times per campaign. If stamp feedback becomes annoying or loses impact, core loop dies.
- **Likelihood**: Medium (common in UI-heavy games with repetitive actions)
- **Impact**: Critical (core loop is the game)
- **Mitigation**:
  - 3+ stamp SFX variants with ±8% pitch/timing variance (reduce fatigue)
  - Animation weight tuning (0.4s ease-out with slight bounce, never instant)
  - Playtest specifically for "does this still feel good at hour 10?" feedback
  - Add subtle animation variance (stamp angle ±2 degrees, impact position ±4px)
- **Contingency**: If playtesting shows fatigue, add "quick stamp" mode (hold Shift for instant feedback, skips animation)
- **Owner**: PIXEL + ECHO

---

### High Risks (Likelihood: Medium, Impact: High)

**Risk #4: Intervention System Feels Arbitrary**

- **Description**: If players can't predict morale impacts or quality outcomes, interventions feel like slot machines instead of strategic choices.
- **Likelihood**: Medium (complex formula with hidden variables)
- **Impact**: High (half the game's depth becomes inaccessible)
- **Mitigation**:
  - Tooltips show exact morale impact before applying intervention
  - Studio archetype descriptions explicitly state sensitivities ("Auteurs hate creative interference: -25 morale")
  - Quality seed impacts are hidden, but morale/trust are visible and predictable
  - Playtest question: "Can you predict intervention outcomes within ±5 morale points?"
- **Contingency**: Add "Intervention Preview" mode that shows exact before/after state (with quality seed still hidden)
- **Owner**: REED + BYTE

**Risk #5: Portfolio Matrix Feels Opaque**

- **Description**: Players don't understand why Prestige/Profit/Populism axes moved, making board evaluations feel unfair.
- **Likelihood**: Medium (multi-variable formulas)
- **Impact**: High (board pressure system loses teeth)
- **Mitigation**:
  - Portfolio tooltip shows detailed breakdown: "Project Titan (Metacritic 91) contributed +18 Prestige"
  - Annual review explicitly cites which games helped/hurt each axis
  - Tutorial explicitly teaches axis definitions and tradeoffs
  - Visual feedback: Axes pulse green when above target, red when below
- **Contingency**: Add "Portfolio Ledger" screen showing full axis contribution history per shipped game
- **Owner**: BYTE + PIXEL

**Risk #6: Switch Performance Below 30 FPS in Handheld Mode**

- **Description**: UI complexity or FMOD audio overhead causes frame drops on Switch hardware.
- **Likelihood**: Low (UI-only game, minimal rendering complexity)
- **Impact**: High (delays ship or forces platform cut)
- **Mitigation**:
  - Profile on Switch devkit by Week 8 (during M3)
  - Reduce UI particle effects if performance is borderline
  - Use texture atlases and minimize draw calls
  - FMOD: Limit concurrent audio streams to 8
- **Contingency**: If 30 FPS unachievable, reduce UI animation complexity or delay Switch port to post-launch (ship PC first)
- **Owner**: BYTE

---

### Medium Risks (Likelihood: Medium, Impact: Medium)

**Risk #7: Studio AI Archetypes Feel Interchangeable**

- **Description**: If archetypes aren't behaviorally distinct, studios feel like random pitch generators instead of persistent characters.
- **Likelihood**: Medium (requires careful balancing and voice writing)
- **Impact**: Medium (long-game investment collapses, but core loop survives)
- **Mitigation**:
  - Archetype behaviors are mechanically distinct (e.g., Auteurs refuse counter-offers, Volatiles have extreme quality variance)
  - Studio email voices are clearly differentiated (Auteur = passionate/pretentious, Reliable = professional/sterile, Volatile = erratic, Fading = nostalgic)
  - Playtest question: "Can you describe the personalities of 3 studios after a 2-hour session?"
- **Contingency**: Add visual archetype identifiers (e.g., Auteur studios have unique pitch deck art style, Volatiles use glitchy transitions)
- **Owner**: NOVA + REED

**Risk #8: Era Events Feel Like Random Noise Instead of Strategic Opportunities**

- **Description**: If era events aren't telegraphed clearly, players feel blindsided instead of challenged.
- **Likelihood**: Medium (rumor system needs clear UI)
- **Impact**: Medium (10-year arc becomes repetitive quarters)
- **Mitigation**:
  - Rumor system triggers 1-2 quarters before major events
  - News feed highlights rumors with distinct visual treatment (orange "RUMOR" badge)
  - Tutorial explicitly teaches: "Rumors let you position ahead of market shifts"
  - Playtest metric: Do 60-70% of players correctly anticipate era shifts?
- **Contingency**: Add "Era Forecast" panel showing upcoming events 4 quarters in advance (reduces surprise, increases strategy)
- **Owner**: REED + PIXEL

---

### Low Risks (Likelihood: Low, Impact: Variable)

**Risk #9: Counter-Offers Become Dominant Strategy**

- **Description**: If counter-offers always save money without proportional quality penalty, full greenlights become suboptimal.
- **Likelihood**: Low (balancing tunable)
- **Impact**: Medium (reduces strategic variety)
- **Mitigation**: Counter-offer acceptance is probabilistic (gated by Trust). Quality seed penalty for budget cuts is significant (30% budget cut = ~15 quality seed penalty). Playtest metric: What % of decisions are counter-offers? Target: <40%.
- **Contingency**: Increase counter-offer quality penalties if playtest shows >40% usage
- **Owner**: REED

**Risk #10: Launch Trailer Doesn't Convert Wishlists**

- **Description**: Trailer fails to communicate hook or looks too niche.
- **Likelihood**: Low (strong concept, clear hook)
- **Impact**: Medium (slower sales ramp, but reviews will carry word-of-mouth)
- **Mitigation**: Follow art direction trailer structure (three-act: Pitch → Consequences → Legacy). Test trailer on focus group before finalizing. Emphasize GIF-worthy moments (stamp coming down, portfolio triangle morphing, Metacritic reveal).
- **Contingency**: Create alternate "developer commentary" trailer if main trailer underperforms
- **Owner**: HYPE

---

## 5. Critical Path Visualization

```
Week 1-4:   [Core Loop Prototype]
                    ↓
Week 5-7:   [Intervention System] ← DEPENDS ON: Core Loop validated
                    ↓
Week 8-10:  [Portfolio & Board] ← DEPENDS ON: Interventions working
                    ↓
Week 11-13: [Era Engine] ← DEPENDS ON: Portfolio logic stable
                    ↓
Week 14-16: [Studio AI] ← DEPENDS ON: Era + Portfolio systems
                    ↓
Week 17-20: [Full Campaign Integration] ← DEPENDS ON: All systems complete
                    ↓
Week 21-24: [Polish & Ports] ← DEPENDS ON: Full campaign playable
                    ↓
Week 25-26: [Launch Prep] ← DEPENDS ON: Platform builds stable
                    ↓
            SHIP (Q4 2026)
```

**Critical Dependencies**:
- Core Loop must validate before building Interventions (Week 4 decision point)
- Interventions must work before Portfolio can be tested (Week 7 dependency)
- Full Campaign Integration must complete before Polish (Week 20 dependency)
- Switch performance must be validated by Week 22 or we delay platform

**Bottleneck Alert**: Weeks 17-20 (Full Campaign Integration) require massive content authoring (50 pitches, 15 studios). REED is bottleneck. Mitigation: NOVA can author pitch narrative text in parallel.

---

## 6. Scope Management

### Feature Pillars (Non-Negotiable for Ship)

These are the five features that define GREENLIGHT. Cut anything else before cutting these.

1. **Greenlight Table** — 50+ authored pitches, quality seed calculation, gut-feel indicator, counter-offers
2. **Intervention System** — 15 interventions with archetype-specific responses, restraint option
3. **Portfolio Matrix** — Prestige/Profit/Populism tracking, board evaluation, annual reviews
4. **Era Engine** — 20 era events spanning 2003-2012, genre/platform demand modifiers, rumor system
5. **Studio Relationships** — 12-15 persistent studios, trust/morale tracking, archetype behaviors, magnum opus unlocks

If we're behind schedule at Week 16, we cut stretch goals, not pillars.

---

### Stretch Goals (Ordered by Impact, Cut from Bottom Up)

1. **Franchise System** — Sequels, spin-offs, franchise fatigue (HIGH IMPACT, but pillar systems support it)
2. **E3 Showfloor Minigame** — Interactive booth allocation, press conference scripting (MEDIUM IMPACT, narrative flavor)
3. **Executive Skill Tree** — Meta-progression, passive abilities (MEDIUM IMPACT, replayability boost)
4. **Rival Publishers** — 2-3 AI competitors bidding on studios (LOW IMPACT, adds complexity without depth)
5. **Developer Documentary Mode** — Post-game retrospective with fake interviews (LOW IMPACT, polish feature)
6. **Sandbox Mode** — Unlimited budget, no board pressure (LOW IMPACT, niche audience)

**Cut Order (if schedule pressure hits)**:
1. Cut Sandbox Mode (Week 18 if we're behind)
2. Cut Developer Documentary (Week 19)
3. Cut Rival Publishers (Week 20)
4. Cut Executive Skill Tree (Week 21)
5. Cut E3 Minigame (Week 22)
6. **DO NOT CUT Franchise System** — This is half-pillar, too integrated to remove cleanly

---

### Cut List (NOT in v1, No Discussion)

These are explicitly out of scope. If anyone asks for them, the answer is "post-launch DLC."

- Playing the games you greenlight (you're the publisher, not the player)
- Real studio/game names (legal nightmare, creative constraint)
- Multiplayer or competitive modes (scope explosion)
- Procedurally generated pitches with no authored backbone (quality collapse)
- Real-time gameplay (this is turn-based, period)
- Post-2012 era content (that's a sequel)

---

## 7. Team Health & Burn Prevention

### No Crunch Policy

I've managed through 6 months of crunch. Two people quit. The game was mediocre. Crunch is a management failure, not a badge of honor. We will not crunch on this project.

**Rules**:
- 40-hour weeks. No weekends except voluntary (and rare).
- If we're behind schedule at Week 16, we cut stretch goals, not sleep.
- Milestones have 1-week buffer built in. That buffer is sacred.
- If someone is working nights, I've failed as producer.

**Warning Signs** (I'm watching for these):
- Team members responding to emails after 8 PM
- Weekend commits becoming regular
- "Just one more sprint" language
- QA reporting bugs faster than dev can fix them

**Intervention**: If I see these signs, I call a retrospective, identify the bottleneck, and cut scope or extend timeline. The game ships when it's ready, not when the calendar says so.

---

### Sprint Retrospective Focus

Every 2-week retrospective will ask:
1. What went well?
2. What blocked us?
3. Are we cutting the right things?
4. Is anyone feeling burned out?

That fourth question is non-negotiable. If the answer is yes, we adjust immediately.

---

## 8. Success Criteria (What "Ship-Ready" Means)

### Technical Ship Criteria (Must Pass All)

- [ ] Full 10-year campaign completable without crashes
- [ ] Save/load stable across sessions (0 corruption in 100 test saves)
- [ ] PC build: 60 FPS UI, <100ms simulation on mid-range hardware
- [ ] Switch build: 30 FPS UI in handheld mode
- [ ] Steam Cloud saves syncing correctly
- [ ] Accessibility: Passes WCAG 2.1 Level AA automated tests
- [ ] No critical bugs (game-breaking, progression-blocking, save-corrupting)

---

### Design Ship Criteria (Must Pass 70%+ in Playtest)

- [ ] Greenlight stamp feels satisfying after 200+ uses (70% tester approval)
- [ ] Players remember at least 1 studio name after 2-hour session (60% recall)
- [ ] Players can predict intervention morale impacts within ±5 points (70% accuracy)
- [ ] Players understand why portfolio axes moved (70% can articulate in post-test)
- [ ] Players cite era positioning as part of their strategy (60% mention it)
- [ ] Campaign completion rate >60% among committed playtesters (12+ hour sessions)

---

### Narrative Ship Criteria (Qualitative)

- [ ] Opening scenario (inherited cancellation) creates tension and teaches core decision weight
- [ ] Studio emails feel distinct per archetype (Auteur ≠ Reliable ≠ Volatile ≠ Fading)
- [ ] Legacy Montage feels emotionally resonant (60% of testers report "reflective" or "impacted")
- [ ] Board feels like institutional pressure, not arbitrary punishment

---

### Audio Ship Criteria

- [ ] Greenlight stamp is the single most satisfying sound in the game
- [ ] Music adapts noticeably to portfolio state (testers can hear board pressure rising)
- [ ] All critical audio has visual redundancy (hearing-impaired accessible)
- [ ] Silence moments (post-cancellation, restraint) feel deliberate, not empty

---

## 9. Dependency Tracking

### External Dependencies (Outside Team Control)

| Dependency | Owner | Risk Level | Mitigation |
|---|---|---|---|
| **Unity 2022 LTS stability** | Unity Technologies | Low | LTS branch is stable, avoid experimental features |
| **Switch devkit access** | Nintendo (via publisher) | Medium | Apply for devkit by Week 1, follow up weekly if delayed |
| **Steam partner approval** | Valve | Low | Standard approval process, 2-week turnaround |
| **Portrait contractor availability** | External artist | Medium | Contract signed by Week 1, milestone-based payment |
| **FMOD licensing** | Firelight Technologies | Low | FMOD Indie (free for <$200K budget) |

**Mitigation Plan**: If Switch devkit delayed beyond Week 8, we develop on PC and port later (post-launch if necessary). Switch is secondary platform, not blocker.

---

### Internal Dependencies (Team Workflow)

| Dependency | Blocker | Blocked Task | Mitigation |
|---|---|---|---|
| ScriptableObject templates | BYTE | Content authoring (REED) | Templates delivered Sprint 1, unblocks REED by Sprint 2 |
| Pitch generation working | BYTE | Pitch concept testing (REED) | Pitch Simulator tool (custom editor) allows offline testing |
| Music composition complete | ECHO | FMOD integration (ECHO) | Music stems delivered incrementally (Year 1-3 first, Year 4-6 later) |
| Full campaign playable | All systems | QA pass (CRASH) | Integration happens Sprint 11-12, QA starts Sprint 12 |

**Critical Dependency**: BYTE's ScriptableObject templates (Sprint 1) unblock REED's content authoring (Sprint 2+). If templates slip, content pipeline stalls. BYTE delivers templates by end of Week 2, non-negotiable.

---

## 10. Post-Launch Support Plan

### Patch 1.1 (Month 2 Post-Launch)

**Scope**: Bug fixes, balance tuning, QoL improvements
- Fix critical bugs reported in first 30 days
- Balance board targets if data shows >50% termination rate
- Add "Undo Last Decision" button (requested QoL feature)
- Fast-forward quarter button (skip quarter-end animations)

**Effort**: 2 weeks (1 programmer, 1 designer)

---

### Patch 1.2 (Month 4 Post-Launch)

**Scope**: Content expansion
- 2-3 new studio archetypes (e.g., "Maverick Publisher," "Corporate Giant")
- 5 new interventions (e.g., "Acquire Studio," "Mandate Live Service")
- Expanded random event pool (20 new events)

**Effort**: 4 weeks (1 programmer, 1 designer, 1 narrative)

---

### Expansion: "The Indie Boom" (Month 8 Post-Launch)

**Scope**: Campaign extension to 2015 (3 additional years, 12 quarters)
- Digital distribution dominance mechanics
- Crowdfunding greenlighting (Kickstarter-era simulation)
- Early Access as intervention option
- 20 new pitches (indie-focused concepts)

**Effort**: 12 weeks (full team, scaled-down)

**Price**: $4.99 DLC or free for early adopters (decide based on base game sales)

---

## 11. Final Notes from CLOCK

This is a data-driven management sim disguised as a narrative experience. The technical complexity is in the simulation layer (12-15 persistent AI entities, 50+ authored concepts, 15 interventions, 40 quarters of state). The emotional complexity is in making players care about spreadsheets because the spreadsheets represent people.

We're not building an engine. We're building an economy. The challenge is managing state interactions without the simulation becoming a black box. Transparency is non-negotiable. If players don't understand why something happened, we've failed.

**What I'm protecting**:
1. The 30-second greenlight loop (if the stamp doesn't feel weighty, the game is dead)
2. The team's health (no crunch, cut scope instead)
3. Save integrity (12-18 hour campaigns mean corruption is catastrophic)

**What I'm watching**:
1. Mid-game pacing sag (Year 5-7 drop-off)
2. Sprint velocity (are we slipping? where's the bottleneck?)
3. Playtest feedback quality (are testers engaged or just checking boxes?)

**What I'll cut without hesitation**:
1. Stretch goals (Sandbox Mode, Developer Documentary)
2. Polish features that don't serve the core loop
3. Platforms if performance doesn't hit targets (Switch is secondary, PC is primary)

**What I won't cut**:
1. The five feature pillars (Greenlight Table, Interventions, Portfolio Matrix, Era Engine, Studio Relationships)
2. The opening scenario (inherited cancellation)
3. The endgame sequences (Legacy Montage, Termination Epilogue)

This game is about the weight of decisions made with incomplete information. Every milestone is a checkpoint where we validate that weight. If the greenlight stamp feels routine by Week 10, we've lost. If players finish the campaign and remember studio names, we've won.

Cut scope, not corners. Ship something real.

---

**Production Plan Status**: Approved for Execution
**Next Milestone**: M1 (Core Loop Prototype) — Week 4 delivery
**First Sprint Planning**: Week 1, Monday 9 AM

— CLOCK
