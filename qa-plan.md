# GREENLIGHT — QA Plan

**QA Lead**: CRASH
**Version**: 1.0
**Date**: 2026-02-07
**Game Type**: Turn-based publisher management simulation
**Target Platforms**: PC (Steam primary), Nintendo Switch (secondary), Mobile tablet (stretch)
**Campaign Length**: 10 years / 40 quarters / 12-18 hours

---

> "A game is its loop. If the 30-second experience isn't compelling, no amount of content saves it."
> — REED
>
> "Every mechanic tells a story."
> — NOVA
>
> "Ship it functional. Profile before optimizing."
> — BYTE
>
> **"Define 'works'."**
> — CRASH

---

## Executive Summary

GREENLIGHT is a systems-heavy management sim with persistent state across 40 quarters, 12-15 AI studio entities, 50+ pitch combinations, and complex formula-driven interactions. The technical complexity is high — quality seeds, trust accumulation, portfolio matrices, intervention cascades, era modifiers — all feeding into emergent outcomes the player never fully sees.

**The core QA risk is this**: A player invests 15 hours into a campaign. One corrupted save, one broken intervention, one unwinnable death spiral, and we lose them forever. The 1-star reviews will mention our names.

**What we're testing**:
1. The 30-second greenlight decision (does it feel meaningful?)
2. The 5-minute management loop (does intervention feel strategic or arbitrary?)
3. The meta campaign (does the 10-year arc hold or sag?)
4. State integrity (save corruption is the mortal sin)
5. Formula correctness (quality seed to Metacritic must be fair, not random)
6. Edge cases (death spirals, exploit loops, studio AI breaking)
7. Platform parity (PC mouse, Switch controller, tablet touch — all must work)

This document defines what "works" means. Every test case here is a question asked with incomplete information and permanent consequences. Just like the game.

---

## 1. Test Plan Overview

### 1.1 Testing Phases

| Phase | Timeline | Focus | Entry Criteria | Exit Criteria |
|-------|----------|-------|----------------|---------------|
| **Alpha** | Weeks 1-8 | Core loop validation. Greenlight Table + Intervention System. | Greenlight stamp functional, 3 studios, 10 pitches. | 30-second loop feels weighty. Interventions modify state correctly. |
| **Beta** | Weeks 9-16 | Full systems integration. Portfolio Matrix, Era Engine, Studio AI. | All core mechanics implemented. Save/load functional. | Full 3-year campaign completable. No blocking bugs. |
| **RC (Release Candidate)** | Weeks 17-20 | Full campaign stability. Edge case hunting. Balance validation. | Full 10-year campaign implemented. All platforms building. | 5 clean full-campaign playthroughs. No crashes, no save corruption. |
| **Platform Cert** | Weeks 21-24 | Platform-specific compliance (Switch TCRs, Steam integration). | All platforms feature-complete. | Platform holder approval. |
| **Launch Prep** | Weeks 25-26 | Day-one patch readiness. Final regression pass. | RC approved. Press builds stable. | Launch build signed off. |

### 1.2 Test Environments

| Environment | Purpose | Configuration |
|-------------|---------|---------------|
| **Dev (PC)** | Daily smoke tests, iteration testing | Unity Editor, Win 10, mouse+keyboard |
| **QA Lab (PC)** | Full regression, performance profiling | Win 10/11, various resolutions (1080p-4K), Steam client |
| **QA Lab (Switch)** | Platform-specific testing, controller, handheld mode | Switch devkit, docked + handheld modes, Pro Controller + Joy-Con |
| **QA Lab (Tablet)** | Touch input, mobile performance | iPad Air (A12), Samsung Galaxy Tab S7, touch only |
| **Automated Test** | Simulation layer unit tests, nightly builds | Headless Unity, Git CI integration |

### 1.3 Test Data Sets

| Data Set | Description | Source |
|----------|-------------|--------|
| **Minimal Campaign** | 3 studios, 10 pitches, 3-year timeline (2003-2005) | Authored by design team |
| **Full Campaign** | 15 studios, 50 pitches, 10-year timeline (2003-2012) | Authored by design team |
| **Chaos Campaign** | Random intervention spam, max active projects, worst-case state complexity | Procedurally generated |
| **Save File Library** | 50+ save files covering Years 1-10, various portfolio states, edge case scenarios | Captured from playtests |

---

## 2. Core Loop Testing — The Greenlight Table (30-Second Loop)

### 2.1 Happy Path

**Preconditions**: New campaign. Year 2003 Q1. 3 studios available. Player has $80M budget.

| Test Case | Steps | Expected Result | Priority |
|-----------|-------|-----------------|----------|
| **GT-001: Greenlight a pitch** | 1. Enter Greenlight Table phase. 2. Review pitch #1 (Studio A, $25M budget). 3. Click GREENLIGHT stamp. | Stamp animation plays (THUNK sound). Studio morale +10. Trust +5. Budget decreases to $55M. Active project created. Project card appears in management panel. | P0 |
| **GT-002: Pass on a pitch** | 1. Enter Greenlight Table. 2. Review pitch #2. 3. Click PASS (red X). | Pitch slides off screen. Studio morale -5. Trust -3. Budget unchanged. Studio may pitch to rival publisher (log event). | P0 |
| **GT-003: Counter-offer (reduced budget)** | 1. Greenlight Table. 2. Select pitch #3. 3. Choose COUNTER-OFFER. 4. Reduce budget by 30% ($20M → $14M). 5. Confirm. | Studio considers offer (based on Trust + Desperation formula). If accepted: morale -5 to -15, quality seed penalized, project created. If rejected: studio trust -8, pitch removed. | P0 |
| **GT-004: Multiple pitches in one quarter** | 1. Greenlight Table with 5 pitches. 2. Greenlight pitch #1. 3. Pass on pitch #2. 4. Counter-offer pitch #3 (accepted). 5. Greenlight pitch #4. 6. Confirm end of phase. | 3 projects created (2 full greenlight, 1 counter-offer). Budget reflects all deductions. Studio relationships updated for all 5 studios (greenlit, passed, counter-offered). | P0 |
| **GT-005: Insufficient budget blocks greenlight** | 1. Budget remaining: $20M. 2. Attempt to greenlight pitch costing $25M. | GREENLIGHT button disabled or error message: "Insufficient budget." Pitch not greenlit. | P1 |
| **GT-006: Active project slot limit** | 1. Campaign with 3 active projects, max slots = 3. 2. Attempt to greenlight 4th pitch. | GREENLIGHT button disabled or error: "No available project slots." Cannot exceed max. | P1 |

### 2.2 Pitch Generation Testing

**Objective**: Validate that pitches generated each quarter are contextually relevant, mechanically sound, and non-repetitive.

| Test Case | Steps | Expected Result | Priority |
|-----------|-------|-----------------|----------|
| **PG-001: Pitch count per quarter** | 1. Advance to Greenlight phase. 2. Count pitches. | 3-5 pitches generated. Never less than 3, never more than 5. | P0 |
| **PG-002: No duplicate studios in one quarter** | 1. Greenlight phase with 5 pitches. 2. Check studio assignments. | Each pitch assigned to a different studio. No studio pitches twice in the same quarter. | P0 |
| **PG-003: Quality seed plausibility** | 1. Generate 20 pitches across multiple quarters. 2. Log quality seeds. | All quality seeds between 20-95. No outliers. Distribution should show variance (not all 70-80). | P1 |
| **PG-004: Gut-feel indicator noise** | 1. Start campaign with Intuition Level 1. 2. Generate pitch with quality seed 75. 3. Check gut-feel display. | Gut-feel displayed between 60-90 (±15 noise). Repeating same studio/concept combination yields different gut-feel values on different runs. | P1 |
| **PG-005: Gut-feel accuracy improves with intuition** | 1. Campaign at Intuition Level 5. 2. Generate pitch with quality seed 80. 3. Check gut-feel. | Gut-feel displayed between 77-83 (±3 noise). Much tighter range than Level 1. | P2 |
| **PG-006: Market fit reflects era** | 1. Year 2006 Q4 (Wii launch). 2. Generate casual game pitch. 3. Check market fit score. | Market fit: High or Very High. Era modifier for casual games should boost fit score. | P1 |
| **PG-007: Studio cooldown (no pitch spam)** | 1. Studio A pitches in Q1. 2. Studio A does not have an active project. 3. Advance to Q2 Greenlight phase. | Studio A does not pitch in Q2 (2-quarter cooldown). Studio A is eligible again in Q3. | P1 |
| **PG-008: Low morale blocks pitching** | 1. Studio B has morale = 10 (below threshold 15). 2. Advance to Greenlight phase. | Studio B does not pitch. Only studios with morale ≥ 15 are eligible. | P1 |
| **PG-009: Destroyed trust blocks pitching** | 1. Studio C has trust = -35 (below threshold -30). 2. Greenlight phase. | Studio C does not pitch. Studio has left or refuses to work with player. | P1 |

### 2.3 Decision Feedback Testing

**Objective**: Validate that player decisions produce immediate, clear, and correct feedback.

| Test Case | Steps | Expected Result | Priority |
|-----------|-------|-----------------|----------|
| **DF-001: Greenlight stamp animation** | 1. Click GREENLIGHT. | Stamp descends with mechanical animation (0.3-0.5 sec). THUNK sound plays. Stamp glows green briefly. Studio rep portrait reacts (smile/nod). | P1 |
| **DF-002: Pass (red X) animation** | 1. Click PASS. | Red X stamp animation. Pitch document slides off-screen. Studio rep portrait reacts (disappointment). No sound or subtle "rejected" SFX. | P1 |
| **DF-003: Budget ticker updates** | 1. Budget display shows $80M. 2. Greenlight $25M pitch. 3. Watch budget display. | Budget animates from $80M → $55M with smooth number lerp (not instant snap). | P2 |
| **DF-004: Active projects panel updates** | 1. Greenlight pitch. 2. Check active projects panel. | New project card appears with: studio name, game concept, budget, timeline (quarters remaining), current phase (pre-production). Card animation: slide in or fade in. | P1 |
| **DF-005: Studio relationship panel updates** | 1. Greenlight pitch from Studio A. 2. Check relationship panel for Studio A. | Trust bar animates +5. Morale bar animates +10. Relationship log shows: "Studio A: Trust +5 (Greenlight approval)." | P1 |
| **DF-006: Counter-offer negotiation feedback** | 1. Counter-offer at 70% budget. 2. Studio accepts. | Brief negotiation beat (0.5-1 sec delay). Studio rep considers, then nods. Confirmation message: "Studio A accepts counter-offer. Morale -10." | P1 |
| **DF-007: Counter-offer rejection feedback** | 1. Counter-offer at 50% budget to low-Trust studio. 2. Studio rejects. | Studio rep shakes head. Message: "Studio B rejects counter-offer. Trust -8." Pitch removed from table. | P1 |

---

## 3. Intervention System Testing (5-Minute Loop)

### 3.1 Milestone Review Flow

**Preconditions**: Campaign has 2 active projects. Advance to Milestone Review phase.

| Test Case | Steps | Expected Result | Priority |
|-----------|-------|-----------------|----------|
| **MS-001: Milestone review triggers** | 1. Greenlight project in Q1. 2. Advance to Q2 Milestone Review phase. | Milestone review screen appears with project status: build snippet, morale report, budget status, risk assessment. | P0 |
| **MS-002: Project phase advancement** | 1. Project starts in pre-production (Q1). 2. Advance quarter. | Project phase advances: Pre-production → Production → Alpha → Beta → Gold. Timeline decrements by 1 quarter each review. | P0 |
| **MS-003: Project ships at gold** | 1. Project reaches Gold phase. 2. Advance to Results phase. | Project ships. Metacritic score generated (quality seed + variance). Revenue calculated. Game appears in shipped games list. | P0 |
| **MS-004: Multiple projects review sequentially** | 1. 3 active projects. 2. Enter Milestone Review phase. | Projects reviewed one at a time. Player sees review #1, makes decision, proceeds to review #2, etc. No simultaneous reviews. | P1 |
| **MS-005: Skip intervention (restraint)** | 1. Milestone review screen. 2. Select "No Intervention" option. | Project continues unchanged. Studio morale +3 (restraint bonus). If Auteur studio: +8 morale. Consecutive restraint bonuses compound (+3, +5, +8, +12). | P0 |

### 3.2 Intervention Mechanics

**Objective**: Validate that each intervention correctly modifies budget, timeline, morale, and quality seed.

| Test Case | Intervention | Preconditions | Expected Result | Priority |
|-----------|--------------|---------------|-----------------|----------|
| **INT-001: Force Multiplayer** | Force Multiplayer | Action game project, mid-production. | Budget +$8M. Timeline +2 quarters. Morale -15 (base). If Auteur studio: morale -25. Quality seed +5 to +15 (genre-dependent, hidden). | P0 |
| **INT-002: Ship It Now** | Ship It Now | Project at Beta phase, 3 quarters remaining. | Timeline set to 0 (ships next quarter). Morale -25. Quality seed locked at current value × 0.6 (heavy penalty). | P0 |
| **INT-003: Expand Scope** | Expand Scope | Project at pre-production. | Budget +$15M. Timeline +3 quarters. Morale +15. Quality seed +10 to +25 (high variance). If Auteur studio: morale +20. If Reliable studio: morale -10. | P0 |
| **INT-004: Delay to Polish** | Delay to Polish | Project at Alpha phase. | Budget +$5M. Timeline +1 quarter. Morale +5. Quality seed +8. If repeated: diminishing returns (+8 → +5 → +2). | P1 |
| **INT-005: Cut Open World** | Cut Open World | Open-world game project. | Budget -$12M. Timeline -2 quarters. Morale -20. Quality seed -10 to +5 (depends on execution risk). If Auteur studio: morale -30. | P1 |
| **INT-006: Cancel Project** | Cancel Project | Any active project. | Project removed from active list. Budget recovers 20% of remaining. Studio morale -40. Trust -20 to -40. All studios with trust < +10: trust -3 (reputation cascade). Portfolio Prestige -10. | P0 |
| **INT-007: Fire Lead Designer** | Fire Lead Designer | Project in crisis (morale < 30). | Budget +$2M (severance). Timeline +1 quarter. Morale -30. Quality seed -15 to +15 (replacement lottery). All Auteur studios: trust -8 (industry-wide reaction). Studio relationship may be destroyed. | P1 |
| **INT-008: Mandate DLC Plan** | Mandate DLC Plan | Project at production phase. | Budget +$3M. Timeline unchanged. Morale -10. Quality seed -3. Post-ship DLC revenue flag enabled (+15-30% lifetime revenue). If Auteur: morale -20. If Reliable: morale +5. | P2 |
| **INT-009: Increase Marketing** | Increase Marketing | Any project. | Budget +$10M (marketing spend, not dev budget). Morale +5. Quality seed unchanged. Post-ship Populism boost. | P2 |
| **INT-010: Archetype sensitivity variance** | Force Multiplayer on Auteur vs. Reliable | Two projects: one Auteur studio, one Reliable studio. Apply "Force Multiplayer" to both. | Auteur studio: morale -25. Reliable studio: morale -5. Both receive same budget/timeline impacts, but morale reaction differs by archetype. | P1 |

### 3.3 Intervention Edge Cases

| Test Case | Steps | Expected Result | Priority |
|-----------|-------|-----------------|----------|
| **INT-E01: Intervention with insufficient budget** | 1. Remaining budget: $5M. 2. Attempt "Expand Scope" (+$15M). | Intervention blocked or error: "Insufficient budget for this intervention." | P0 |
| **INT-E02: Multiple interventions on one project** | 1. Project A: Apply "Delay to Polish" in Q2. 2. Apply "Force Multiplayer" in Q3. | Both interventions stack. Budget reflects cumulative costs. Morale reflects cumulative penalties. Quality seed modifiers accumulate. | P0 |
| **INT-E03: Restraint after intervention** | 1. Intervene in Q2. 2. Restrain in Q3. | Restraint bonus still applies (+3 morale), but does not undo previous intervention effects. | P1 |
| **INT-E04: Consecutive restraint compounding** | 1. Restrain Q1 (+3 morale). 2. Restrain Q2 (+5 morale). 3. Restrain Q3 (+8 morale). 4. Restrain Q4 (+12 morale). | Morale bonuses compound each consecutive quarter. If player intervenes in Q5, restraint streak resets. | P1 |
| **INT-E05: Board flags restraint on crisis project** | 1. Project with budget overrun > 30% and morale < 25. 2. Choose restraint (no intervention). | Board Confidence -5. Warning message: "Negligent management." Player penalized for ignoring crisis. | P2 |
| **INT-E06: Ship Early intervention on pre-production** | 1. Project at pre-production (not yet shippable). 2. Attempt "Ship It Now." | Intervention blocked or catastrophic quality penalty. Game cannot ship before Alpha. | P1 |
| **INT-E07: Cancel project mid-development (Year 2 of 3)** | 1. Project 12 quarters into 18-quarter timeline. 2. Cancel. | Budget recovers ~6 quarters × budget = 20% of total remaining. Studio morale -40. Trust -30 (moderate completion percentage). Inter-studio trust cascade triggers. | P1 |

---

## 4. Portfolio Matrix & Board Evaluation Testing

### 4.1 Portfolio Axis Calculations

**Objective**: Validate that shipped games correctly modify Prestige, Profit, and Populism axes.

| Test Case | Game Scenario | Expected Portfolio Impact | Priority |
|-----------|---------------|---------------------------|----------|
| **PM-001: High-prestige indie hit** | Metacritic 94, $8M budget, 200K units sold, Auteur studio. | Prestige: +20 to +25. Profit: -5 to 0 (break-even or slight loss). Populism: +5 (niche audience). | P0 |
| **PM-002: Blockbuster sequel** | Metacritic 82, $30M budget, 3M units sold, franchise 2nd installment. | Prestige: +5 (decent but not groundbreaking). Profit: +20 to +25 (high ROI). Populism: +15 to +20 (mass-market). | P0 |
| **PM-003: Critically panned shovelware** | Metacritic 62, $15M budget, 5M units sold (licensed IP). | Prestige: -5 to -10. Profit: +15 (profitable despite quality). Populism: +12 (wide reach). | P0 |
| **PM-004: Commercial disaster** | Metacritic 91, $40M budget, 150K units sold. | Prestige: +18 to +22 (critical acclaim). Profit: -25 (massive loss). Populism: +3 (limited audience). | P1 |
| **PM-005: Multiple games in one quarter** | Ship 3 games in Q4: prestige darling, reliable sequel, casual hit. | All three games' axis impacts stack. Portfolio matrix shows net change from all three. Final values = previous + Δ1 + Δ2 + Δ3. | P1 |
| **PM-006: Axis clamping (0-100)** | Prestige currently 95. Ship game with +10 Prestige impact. | Prestige clamped at 100 (not 105). No overflow. Same for all axes. | P1 |

### 4.2 Board Target Evaluation

**Objective**: Validate that board evaluates portfolio against targets and applies consequences correctly.

| Test Case | Year | Portfolio State | Board Targets | Expected Board Response | Priority |
|-----------|------|-----------------|---------------|-------------------------|----------|
| **BE-001: Exceeds all targets** | Year 2 | Prestige: 50, Profit: 60, Populism: 55 | P: 35, Pr: 35, Po: 35 | Satisfaction > 110. "Exceptional." Budget +10% next year. Bonus greenlight slot. | P0 |
| **BE-002: Meets all targets** | Year 3 | Prestige: 42, Profit: 45, Populism: 38 | P: 40, Pr: 40, Po: 35 | Satisfaction 90-110. "Satisfactory." No changes. | P0 |
| **BE-003: One axis below target (1st warning)** | Year 4 | Prestige: 35, Profit: 50, Populism: 42 | P: 40, Pr: 45, Po: 40 | Satisfaction 70-89. Warning issued. "Prestige slipping." Top-tier studios reduce pitch frequency 25%. | P0 |
| **BE-004: One axis below target (2nd consecutive)** | Year 5 | Prestige: 38, Profit: 52, Populism: 43 | P: 45, Pr: 50, Po: 45 | Consequence triggered. Auteur + Volatile studios stop pitching entirely. Award show invites revoked. | P1 |
| **BE-005: One axis below target (3rd consecutive)** | Year 6 | Prestige: 40, Profit: 56, Populism: 46 | P: 45, Pr: 55, Po: 45 | Crisis. Board forces acquisition of prestige studio at 2× cost ($30M deducted). | P1 |
| **BE-006: Profit crash (2 consecutive years)** | Year 5-6 | Profit < target both years. | Varies | Budget cut 25%. One active project slot removed. Player can manage fewer projects. | P1 |
| **BE-007: Satisfaction < 50 (termination risk)** | Year 7 | Prestige: 30, Profit: 40, Populism: 35 | P: 50, Pr: 55, Po: 50 | Satisfaction < 50. Yellow card. If next year also < 50: termination (game over). | P0 |
| **BE-008: Termination (fired)** | Year 8 | Two consecutive years satisfaction < 50. | Varies | Termination epilogue triggered. Montage of studios/games post-player departure. Campaign ends. | P0 |

### 4.3 Board Edge Cases

| Test Case | Steps | Expected Result | Priority |
|-----------|-------|-----------------|----------|
| **BE-E01: Year 1 grace period** | Year 1 with mediocre portfolio (all axes ~30). | No consequences. Year 1 targets are lenient (P: 30, Pr: 30, Po: 30). Board gives new exec room to learn. | P1 |
| **BE-E02: Target escalation per year** | Compare Year 1 targets to Year 10 targets. | Year 1: P: 30, Pr: 30, Po: 30. Year 10: P: 55, Pr: 65, Po: 55. Targets increase each year per design doc table. | P1 |
| **BE-E03: Specialization demand (Year 7+)** | Year 7: At least one axis must exceed 70. Player has P: 52, Pr: 58, Po: 54 (all below 70). | Board flags specialization failure. Warning: "No clear identity." Targets increase +5 next year. | P2 |
| **BE-E04: Portfolio recovery after crisis** | Year 5 crisis (Prestige crash). Year 6 player ships 2 high-Prestige games. Prestige recovers to 72. | Crisis status cleared. Board satisfaction improves. Studio pitch frequency recovers. Consequences can be reversed if player course-corrects. | P1 |

---

## 5. Studio AI & Relationship Testing

### 5.1 Trust Accumulation

**Objective**: Validate that trust modifies correctly based on player actions and persists across campaign.

| Test Case | Action Sequence | Expected Trust Impact | Priority |
|-----------|-----------------|----------------------|----------|
| **TR-001: Greenlight at full budget** | Studio A: Greenlight pitch at $25M (full budget). | Trust +5. | P0 |
| **TR-002: Greenlight at reduced budget** | Studio A: Counter-offer accepted at 70% budget. | Trust -5 (moderate cut). | P0 |
| **TR-003: Pass on pitch** | Studio A pitches. Player passes. | Trust -3. | P0 |
| **TR-004: Cancel project mid-development** | Studio A working on project for 6 quarters. Player cancels. | Trust -30 (significant investment wasted). | P0 |
| **TR-005: Ship a hit (80+ Metacritic)** | Studio A's game ships with Metacritic 87. | Trust +8. | P0 |
| **TR-006: Ship a flop (<60 Metacritic)** | Studio A's game ships with Metacritic 52. | Trust -5 (studio blames publisher pressure). | P1 |
| **TR-007: Intervention on project** | Apply "Force Multiplayer" to Studio A's project. | Trust -1 to -3 (depends on intervention type and archetype). | P1 |
| **TR-008: Restraint (no intervention)** | Milestone review. Choose restraint. | Trust +3 (Auteur: +8). Consecutive restraint compounds trust gains. | P1 |
| **TR-009: Fire Lead Designer** | Fire Lead Designer intervention on Studio A's project. | Trust -20 to -40 (devastating). All Auteur studios industry-wide: trust -8. | P1 |
| **TR-010: Trust accumulation over 10 years** | Studio A: Multiple greenlights, restraint, shipped hits across 10 years. | Trust can reach +50 (max). High trust unlocks "magnum opus" pitch. Trust history persists. | P1 |

### 5.2 Studio Archetype Behavior

**Objective**: Validate that each archetype behaves according to design spec.

| Archetype | Test Case | Expected Behavior | Priority |
|-----------|-----------|-------------------|----------|
| **Auteur** | AU-001: Intervention sensitivity | Apply "Demand Sequel Hook" intervention. | Morale penalty doubled (base -10 → -20 for Auteur). High sensitivity to creative interference. | P0 |
| **Auteur** | AU-002: Restraint reward | No intervention across 3 consecutive milestones. | Morale +8 per restraint (vs. +3 base). Auteurs reward trust. | P0 |
| **Auteur** | AU-003: Magnum opus unlock | Trust exceeds +30. Year ≥ 2007. | Studio pitches exclusive "magnum opus" concept (high-risk, high-quality ceiling). Pitch flagged visually. | P0 |
| **Auteur** | AU-004: Destroyed relationship** | Trust drops below -20. | Studio sends scathing email, leaves publisher, industry press story appears. Prestige -10. Studio permanently removed from pitch pool. | P1 |
| **Reliable** | RE-001: Consistent quality** | Ship 3 games from Reliable studio. | Metacritic scores: 72, 78, 75 (narrow range 70-82). Predictable quality, low variance. | P0 |
| **Reliable** | RE-002: Intervention tolerance | Apply 3 interventions to one project (Force Multiplayer, Mandate DLC, Delay to Polish). | Morale penalties minimal (-5 each vs. -15 base). Reliable studios tolerate structural mandates. | P0 |
| **Reliable** | RE-003: Franchise expansion unlock | Trust > +25 with Reliable studio. Studio has shipped 2+ games. | Studio offers franchise expansion / spinoff pitch at reduced budget (80% of baseline). | P1 |
| **Volatile** | VO-001: Quality variance | Ship 3 games from Volatile studio with minimal intervention. | Metacritic scores show wide variance (e.g., 48, 88, 63). Quality unpredictable. | P0 |
| **Volatile** | VO-002: Morale swings | Check Volatile studio morale across 4 quarters. | Morale swings ±20 per quarter. High volatility. Morale can go from 70 to 50 to 75 without player intervention. | P1 |
| **Volatile** | VO-003: Capability growth on hit | Volatile studio ships game with Metacritic 85+. | Studio Capability permanently +10. Studio "finds their voice." Future pitches have higher quality ceiling. | P1 |
| **Volatile** | VO-004: Implosion on micromanagement | Apply 4+ interventions to Volatile studio's project. | Morale craters. Studio sends "We're done" email. Project cancelled by studio. Studio dissolves. Reputation cascade. | P1 |
| **Fading** | FA-001: Capability decay** | Fading studio across 5 years with no hits. | Capability decreases 5 points per year (e.g., 80 → 75 → 70 → 65 → 60). Quality ceiling drops over time. | P0 |
| **Fading** | FA-002: Reinvention path | Greenlight Fading studio's NEW IP (not sequel). Game scores 82+. | Capability decay halts and reverses (+3 per year). Studio reinvented. | P1 |
| **Fading** | FA-003: Graceful retirement | Fading studio capability drops below 40. No reinvention. | Studio sends retirement email. Studio removed from pitch pool. Narrative event: "Titan Forge closes doors after 15 years." | P2 |

### 5.3 Inter-Studio Reputation Cascades

**Objective**: Validate that major actions trigger industry-wide studio reactions.

| Test Case | Action | Expected Cascade | Priority |
|-----------|--------|------------------|----------|
| **RE-001: Cancel project (reputation hit)** | Cancel Studio A's project. All studios with trust < +10. | All low-trust studios: trust -3. Log event: "Studios are wary after [Studio A] cancellation." | P1 |
| **RE-002: Ship critically acclaimed game** | Ship 90+ Metacritic game. All studios of same archetype. | Studios of same archetype: trust +5. "They understand people like us." | P1 |
| **RE-003: Fire Lead Designer (Auteur backlash)** | Fire Lead Designer intervention on any project. | All Auteur studios: trust -8 regardless of relationship. Industry-wide reaction to perceived cruelty. | P1 |
| **RE-004: Multiple cancellations** | Cancel 3 projects in one year. | Prestige -30 cumulative. Multiple low-trust studios stop pitching. "Reputation for killing projects" industry rumor. | P2 |

---

## 6. Era Engine & Historical Event Testing

### 6.1 Scheduled Era Events

**Objective**: Validate that era events trigger on schedule and apply correct modifiers.

| Event | Trigger | Expected System Changes | Priority |
|-------|---------|-------------------------|----------|
| **EE-001: Xbox Live (2003)** | Year 2003, any quarter | Games with multiplayer: quality seed +5. Xbox platform: install base growth +10%. | P0 |
| **EE-002: Xbox 360 Launch (2005)** | Year 2005 Q4 | Next-gen projects: quality seed +10 if polished, -15 if rushed. Next-gen budgets: +30%. PS2 revenue-per-unit declining. | P0 |
| **EE-003: Wii Launch (2006)** | Year 2006 Q4 | Casual/party games: Populism delta +20. Wii install base growth 3× Xbox 360 rate. Board demands "casual strategy response" within 2 quarters. | P0 |
| **EE-004: Metacritic Influence (2005)** | Year 2005 | Revenue formula adds: (Metacritic - 70) × 0.02 multiplier. Review scores now directly affect revenue more strongly. | P1 |
| **EE-005: Digital Distribution (2007)** | Year 2007 | Digital-only budget games unlocked. Budget cap $5M, 70% margin. Indie/Volatile studios can pitch digital-only projects. | P1 |
| **EE-006: DLC Economy (2008)** | Year 2008 | "Mandate DLC Plan" intervention unlocked. DLC revenue: +15-30% post-launch per title. | P1 |
| **EE-007: Recession (2008)** | Year 2008 | Annual budget -15%. Board Profit expectations frozen (no increase this year). Studios' Desperation +15 industry-wide. | P0 |
| **EE-008: Mobile Gaming (2009)** | Year 2009 | Board asks "What's our mobile strategy?" Ignoring mobile: -5 Populism per year going forward. Mobile project slot unlocked. | P2 |

### 6.2 Era Event Impact Validation

| Test Case | Scenario | Expected Outcome | Priority |
|-----------|----------|------------------|----------|
| **EE-V01: Casual game before vs. after Wii** | Ship casual game in 2005 (pre-Wii) and identical casual game in 2007 (post-Wii). | 2005 game: Moderate Populism, standard revenue. 2007 game: +20 Populism, revenue +25% due to era demand multiplier. | P1 |
| **EE-V02: Next-gen rushed project** | Greenlight Xbox 360 game in Q1 2005. Force "Ship It Now" in Q4 2005 (launch window). | Quality seed -15 (rushed next-gen penalty). Metacritic likely 60s. Revenue boost from launch window partially offsets quality loss. | P1 |
| **EE-V03: Digital-only pitch availability** | Year 2006 (pre-digital distribution). Check for digital-only pitches. | No digital-only pitches. Year 2007 (post-event): Digital-only pitches appear from Indie/Volatile studios. | P1 |
| **EE-V04: Recession budget impact** | Year 2007 budget: $140M. Year 2008 (recession). | Year 2008 budget: $119M (15% cut). Player must adjust greenlight strategy. | P0 |
| **EE-V05: Board casual strategy demand (Wii)** | Wii launches Q4 2006. Ignore casual games for 2 quarters (Q1-Q2 2007). | Board warning: "Casual market strategy required." If ignored for 4 quarters: consequences (budget cut or forced greenlight mandate). | P2 |

### 6.3 Rumor System (Advance Warning)

**Objective**: Validate that major events are telegraphed 1-2 quarters early.

| Test Case | Event | Rumor Timing | Expected Rumor Text | Priority |
|-----------|-------|--------------|---------------------|----------|
| **RU-001: Wii launch rumor** | Wii Launch (2006 Q4) | 2006 Q2-Q3 | "Sources say Nintendo is developing a motion-control console for 2006 holiday launch." Appears in news feed. | P1 |
| **RU-002: Xbox 360 rumor** | Xbox 360 Launch (2005 Q4) | 2005 Q2 | "Microsoft confirms next-gen Xbox for fall 2005. Developers predict $30M+ budgets." | P1 |
| **RU-003: Recession rumor** | Recession (2008) | 2007 Q4 | "Economic analysts predict downturn. Game publishers may face budget constraints." | P2 |
| **RU-004: Player positioning based on rumor** | See Wii rumor in Q2 2006. Greenlight casual game in Q3 2006 for Q1 2007 ship. | Game ships post-Wii launch. Benefits from era demand boost. Player feels clever for anticipating. | P1 |

---

## 7. Save System Testing (The Mortal Sin)

### 7.1 Save/Load Integrity

**Objective**: Save corruption is catastrophic. Test exhaustively.

| Test Case | Steps | Expected Result | Priority |
|-----------|-------|-----------------|----------|
| **SV-001: Manual save** | 1. Campaign Year 3 Q2. 2. Open menu. 3. Manual save to slot 1 "MyGame". | Save file created at persistentDataPath/Saves/MyGame.json. File size ~500KB. No errors. | P0 |
| **SV-002: Load manual save** | 1. Load "MyGame" save. | Campaign state restored exactly: Year 3 Q2, same studios, same projects, same budget, same portfolio. No data loss. | P0 |
| **SV-003: Auto-save at quarter end** | 1. Complete quarter (greenlight + milestones + results). 2. Confirm quarterly summary. | Auto-save triggers. File: autosave_2005_Q2.json. Player did not explicitly save, but state is checkpointed. | P0 |
| **SV-004: Auto-save rotation (max 5)** | 1. Play through 8 quarters. 2. Check auto-save folder. | Only 5 most recent auto-saves exist (Q4, Q5, Q6, Q7, Q8). Older auto-saves (Q1-Q3) deleted automatically. | P1 |
| **SV-005: Multiple save slots** | 1. Save to slot 1 (Year 2). 2. Continue to Year 5. 3. Save to slot 2 (Year 5). 4. Load slot 1. | Slot 1 loads Year 2 state. Slot 2 unchanged. Multiple independent campaigns supported. | P0 |
| **SV-006: Overwrite existing save** | 1. Save slot 1 (Year 3). 2. Continue to Year 4. 3. Save slot 1 again (overwrite). | Slot 1 now contains Year 4 state. Year 3 state is lost (expected). No corruption. | P1 |
| **SV-007: Save during decision phase** | 1. Greenlight Table with 3 pitches. 2. Greenlight pitch #1. 3. Save before confirming end of phase. | Save captures partial quarter state. On load, player resumes at Greenlight Table with pitch #1 already greenlit, pitches #2-3 still available. | P1 |
| **SV-008: Load after intervention** | 1. Milestone review. 2. Apply "Expand Scope" intervention. 3. Save. 4. Quit. 5. Load. | Project state reflects intervention: budget +$15M, timeline +3 quarters, morale +15. Intervention history logged. | P1 |
| **SV-009: State consistency check** | 1. Save Year 10 campaign (max complexity: 15 studios, 50 shipped games, 6 active projects). 2. Load. 3. Validate all state. | All studios have correct capability/morale/trust/reputation. All projects have correct budget/timeline/quality seed. All shipped games in history. No orphaned references. | P0 |

### 7.2 Save Corruption & Recovery

**Objective**: Detect corrupt saves and recover gracefully.

| Test Case | Steps | Expected Result | Priority |
|-----------|-------|-----------------|----------|
| **SV-C01: Corrupted JSON (manual edit)** | 1. Save game. 2. Manually edit .json file: delete random lines. 3. Attempt load. | Load fails with error: "Save file corrupted." Offer option to load most recent auto-save. Player loses minimal progress. | P0 |
| **SV-C02: Invalid year value** | 1. Edit save: set currentYear = 2050. 2. Load. | Validation fails: year out of range (2003-2012). Load aborted. Fallback to auto-save. | P1 |
| **SV-C03: Orphaned project (studio missing)** | 1. Edit save: delete studio ID from studios array, but project still references it. 2. Load. | Validation detects orphaned project. Error: "Referential integrity failure." Load aborted. | P1 |
| **SV-C04: Negative budget** | 1. Edit save: set remainingBudget = -50000000. 2. Load. | Validation fails: budget cannot be negative. Load aborted. | P1 |
| **SV-C05: Power loss mid-save (crash simulation)** | 1. Initiate save. 2. Kill Unity process during File.Write (simulate crash/power loss). 3. Restart. 4. Attempt load. | .tmp file exists but .json incomplete. Load detects corruption. Falls back to previous auto-save. Player warned: "Last save incomplete, loading previous auto-save." | P0 |
| **SV-C06: Auto-save fallback chain** | 1. Corrupt manual save. 2. Corrupt autosave_Q8. 3. Load game. | System attempts manual save → fails. Attempts autosave_Q8 → fails. Falls back to autosave_Q7 → succeeds. Player loses 1 quarter. | P1 |

### 7.3 Save Migration (Version Updates)

**Objective**: Old saves work after game updates.

| Test Case | Save Version | Game Version | Expected Migration | Priority |
|-----------|--------------|--------------|-------------------|----------|
| **SV-M01: v1.0 save loaded in v1.1** | v1.0 save (no DLC system) | v1.1 (DLC system added) | Migration runs: all projects.hasDLCPlan set to false (default). Save upgraded to v1.1 format. No data loss. | P0 |
| **SV-M02: Migration logs version change** | v1.0 save | v1.1 | Console log: "Migrating save from v1.0 to v1.1." Save file saveVersion field updates: 1 → 1.1. | P2 |
| **SV-M03: Multi-step migration (v1.0 → v1.3)** | v1.0 save | v1.3 (3 updates released) | Migration runs sequentially: v1.0 → v1.1 → v1.2 → v1.3. All migration steps applied. Save functional in v1.3. | P1 |
| **SV-M04: Future-proof migration failure** | v2.0 save (hypothetical future version) | v1.3 (current) | Load fails gracefully: "Save file from newer version. Please update game." No crash. | P2 |

### 7.4 Cloud Save (PC Steam)

**Objective**: Steam Cloud saves sync across devices.

| Test Case | Steps | Expected Result | Priority |
|-----------|-------|-----------------|----------|
| **CS-001: Save uploads to Steam Cloud** | 1. PC A: Save game. 2. Quit. 3. Check Steam Cloud sync indicator. | Save file uploaded. Steam Cloud shows "synced" status. | P1 |
| **CS-002: Download save on different PC** | 1. PC A: Save game. 2. PC B: Launch game (same Steam account). | Steam Cloud downloads save. Save appears in load menu on PC B. Load succeeds. | P1 |
| **CS-003: Conflict resolution (newer save)** | 1. PC A: Save (timestamp 10:00 AM). 2. PC B (offline): Save (timestamp 10:05 AM, newer). 3. PC B goes online. | Steam Cloud detects conflict. Asks player: "Keep local (newer) or cloud (older)?" Player chooses. No automatic overwrite. | P2 |
| **CS-004: Cloud save disabled offline** | 1. PC offline (no internet). 2. Save game. | Save stored locally only. When online, save uploads to Steam Cloud on next sync. | P2 |

---

## 8. Performance & Optimization Testing

### 8.1 Target Performance Benchmarks

| Platform | Target FPS | Target Turn Time | Max Load Time | Priority |
|----------|------------|------------------|---------------|----------|
| **PC (mid-range)** | 60 FPS (UI) | <100ms (quarter advance) | <3 sec (campaign load) | P0 |
| **Nintendo Switch** | 30 FPS (UI, handheld) | <200ms (quarter advance) | <5 sec (campaign load) | P0 |
| **Tablet (iPad Air)** | 30 FPS (UI) | <300ms (quarter advance) | <4 sec (campaign load) | P2 |

### 8.2 Performance Test Cases

| Test Case | Scenario | Measurement | Pass Criteria | Priority |
|-----------|----------|-------------|---------------|----------|
| **PF-001: UI frame rate (PC)** | 1. Greenlight Table with 5 pitches. 2. Rapid mouse movement over UI. 3. Profile frame rate. | FPS counter over 60 seconds. | Average ≥60 FPS. No frame drops below 55 FPS. | P0 |
| **PF-002: UI frame rate (Switch handheld)** | 1. Same UI stress test on Switch handheld mode. | FPS counter. | Average ≥30 FPS. No drops below 25 FPS. | P0 |
| **PF-003: Quarter advance time (worst-case)** | 1. Year 10 Q4 (max complexity: 15 studios, 6 active projects, 50 shipped games). 2. Advance quarter. 3. Time from "Confirm" click to next phase UI. | Stopwatch. | PC: <100ms. Switch: <200ms. | P0 |
| **PF-004: Pitch generation time** | 1. Generate 5 pitches (15 studios, 50 concepts). 2. Profile PitchGenerator. | Unity Profiler. | <10ms on PC. <20ms on Switch. | P1 |
| **PF-005: Save file write time** | 1. Year 10 campaign (2MB save file). 2. Initiate manual save. 3. Time until "Save complete" message. | Stopwatch. | <50ms on PC. <100ms on Switch. | P1 |
| **PF-006: Memory footprint** | 1. Full campaign running. 2. Check total memory usage. | Unity Profiler Memory module. | <150MB on PC. <200MB on Switch. | P1 |
| **PF-007: GC allocations (per-frame)** | 1. Greenlight Table UI open. 2. Profile 60 seconds of idle UI. 3. Check GC allocations. | Unity Profiler. | Zero per-frame allocations. UI uses object pooling for dynamic elements. | P2 |
| **PF-008: Load time (Year 10 save)** | 1. Load 2MB save file (Year 10 full campaign). 2. Time from "Load" click to campaign screen. | Stopwatch. | PC: <3 sec. Switch: <5 sec. | P1 |

### 8.3 Stress Testing

**Objective**: Find breaking points under extreme conditions.

| Test Case | Scenario | Expected Outcome | Priority |
|-----------|----------|------------------|----------|
| **ST-001: Spam greenlight decisions** | 1. Greenlight Table. 2. Spam-click GREENLIGHT 100 times in 10 seconds. | UI handles rapid input. No crashes. No duplicate greenlights. Last valid input wins. | P1 |
| **ST-002: Max active projects** | 1. Campaign with 6 active projects (max). 2. Advance 10 quarters (60 milestone reviews total). | All reviews process correctly. No slowdown. No crashes. | P1 |
| **ST-003: Chaos mode (random interventions)** | 1. Bot applies random interventions to all projects for 40 quarters. | Campaign completes without crashes. Edge cases surface (e.g., negative morale clamped at 0, quality seed variance extremes). | P2 |
| **ST-004: Rapid save/load cycling** | 1. Save. 2. Load. 3. Repeat 50 times in 5 minutes. | No crashes. No save corruption. No memory leaks. | P1 |
| **ST-005: All studios destroyed** | 1. Cancel all projects. 2. Fire lead designers on remaining studios. 3. All studios leave (trust < -30). | Campaign continues with empty studio pool. Board fires player for negligence (termination ending). No crash. | P2 |

---

## 9. Edge Case & Exploit Testing

### 9.1 Death Spirals

**Objective**: Validate that players can enter unwinnable states, but they're telegraphed clearly.

| Test Case | Scenario | Expected Player Experience | Priority |
|-----------|----------|----------------------------|----------|
| **DS-001: Budget exhaustion** | 1. Greenlight 4 expensive projects ($100M total, budget $80M). 2. Cannot afford interventions. | Player warned before greenlighting: "Insufficient budget." If player exhausts budget, they cannot greenlight new projects until revenue arrives. Board may cut budget further if Profit crashes. | P1 |
| **DS-002: Morale death spiral** | 1. Apply 5 harsh interventions to one project. 2. Morale drops to 5. 3. Quality seed crashes. 4. Game ships with Metacritic 42. | Studio morale penalty applies: quality seed penalized by (30 - morale) × 0.5 = ~12 points. Game flops. Studio trust -5. Prestige -10. Player sees consequences of over-intervention. | P1 |
| **DS-003: Board termination spiral** | 1. Year 5-6: ignore Prestige entirely (only greenlight shovelware). 2. Prestige crashes below target for 3 consecutive years. | Year 5: Warning. Year 6: Auteur studios stop pitching. Year 7: Forced acquisition ($30M). Year 8: If still failing, termination. Player has 3 years to course-correct. | P1 |
| **DS-004: All studios lost** | 1. Cancel 5 projects in 2 years. 2. All studios' trust drops below -30. 3. No studios pitch. | Greenlight Table phase has 0 pitches. Board fires player within 2 quarters for "inability to attract talent." Termination ending. | P2 |
| **DS-005: Recovery from spiral** | 1. Prestige death spiral (3 consecutive years below target). 2. Year 7: Player ships 2 high-Prestige games (Metacritic 90+). 3. Prestige recovers to 75. | Crisis status cleared. Board satisfaction improves. Studio pitch frequency recovers. Death spirals are recoverable if player acts aggressively. | P1 |

### 9.2 Exploits & Loopholes

**Objective**: Find and patch unintended optimal strategies.

| Test Case | Exploit Attempt | Expected Prevention | Priority |
|-----------|-----------------|---------------------|----------|
| **EX-001: Counter-offer spam** | 1. Only use counter-offers (70% budget). 2. Never greenlight at full budget. | Counter-offers have acceptance probability gated by Trust + Desperation. Low-trust studios reject 50% of the time. Quality seed penalty for budget cuts is significant (30% cut = ~15 quality seed loss). Not a dominant strategy. | P0 |
| **EX-002: Restraint farming (morale exploit)** | 1. Never intervene. 2. Farm consecutive restraint bonuses (+3, +5, +8, +12). 3. Studio morale maxes at 100. | Restraint bonus caps at +12 (4th consecutive). If project enters crisis (budget overrun > 30%, morale < 25), restraint triggers board penalty (-5 Board Confidence). Restraint is strategic, not exploitable. | P1 |
| **EX-003: Cancel-early exploit (budget recovery)** | 1. Greenlight project ($25M). 2. Immediately cancel (recover 20% of remaining = ~$23M). 3. Repeat. | Cancellation inflicts heavy trust/morale penalties. All studios with trust < +10: trust -3. Prestige -10 per cancel. Reputation cascade makes future pitches scarce. Exploit is self-defeating. | P1 |
| **EX-004: Sequel spam (franchise exploit)** | 1. Ship successful game. 2. Greenlight sequel. 3. Greenlight sequel to sequel. 4. Repeat for 5 installments. | Franchise fatigue applies: each installment gets cumulative -3 quality seed and -5% revenue. By 4th installment, franchise is coasting on brand alone with declining returns. Forces player to invest in new IP. | P0 |
| **EX-005: DLC money printing** | 1. Mandate DLC on all projects. 2. Farm +15-30% revenue per title. | DLC mandate costs morale (-10 base, -20 for Auteurs). Splits dev focus (quality seed -3). DLC revenue boost is real but has quality/morale costs. Balanced trade-off. | P1 |
| **EX-006: Auteur trust farming (magnum opus)** | 1. Greenlight Auteur studio at full budget. 2. Never intervene for 3 years. 3. Farm trust to +30. 4. Unlock magnum opus. 5. Repeat with all Auteurs. | Magnum opus is one-time unlock per studio (hasPitchedMagnumOpus flag). Auteur studios are rare (3-4 in pool). Strategy is viable but limited by studio scarcity. Intentional design. | P2 |

---

## 10. Platform-Specific Testing

### 10.1 PC (Steam) Testing

| Test Case | Feature | Steps | Expected Result | Priority |
|-----------|---------|-------|-----------------|----------|
| **PC-001: Mouse input precision** | UI clicking | 1. Greenlight Table. 2. Click pitch cards, buttons, tooltips rapidly. | All clicks register. No input loss. Hover states work. | P0 |
| **PC-002: Keyboard shortcuts** | Hotkeys | 1. Press ESC (menu). 2. Press Space (confirm). 3. Press Tab (cycle UI panels). | Shortcuts work. Configurable in settings. | P2 |
| **PC-003: Multi-monitor support** | Display | 1. Run game on secondary monitor. 2. Alt-Tab. | Game maintains resolution and position. No black screen on focus loss. | P1 |
| **PC-004: 4K resolution scaling** | UI scaling | 1. Set resolution to 3840×2160. 2. Check UI clarity. | UI scales correctly. Text readable. No pixelation. Canvas Scaler works. | P1 |
| **PC-005: Steam achievements** | Integration | 1. Complete Year 1. 2. Trigger achievement: "First Year Complete." | Achievement unlocks. Steam overlay shows notification. | P1 |
| **PC-006: Steam Cloud save sync** | Cloud save | 1. Save on PC A. 2. Load on PC B. | Save syncs. Campaign state identical. | P1 |

### 10.2 Nintendo Switch Testing

| Test Case | Feature | Steps | Expected Result | Priority |
|-----------|---------|-------|-----------------|----------|
| **SW-001: Controller button mapping** | Input | 1. Use Pro Controller. 2. Navigate Greenlight Table. 3. A = confirm, B = cancel, D-pad = navigate. | All buttons respond. No input lag. Button prompts show Switch glyphs (A/B/X/Y). | P0 |
| **SW-002: Joy-Con support** | Input | 1. Use Joy-Con (single or paired). 2. Navigate full quarter. | Same functionality as Pro Controller. No drift issues (calibration menu accessible). | P0 |
| **SW-003: Handheld mode (720p)** | Performance | 1. Switch to handheld mode (undocked). 2. Play full quarter. 3. Profile FPS. | 30 FPS maintained. No frame drops. UI readable at 720p. | P0 |
| **SW-004: Docked mode (1080p)** | Performance | 1. Docked mode. 2. Full quarter. 3. Profile FPS. | 30 FPS maintained (same as handheld). Optional: 60 FPS if performance headroom exists. | P1 |
| **SW-005: Sleep mode (suspend/resume)** | System | 1. Campaign active. 2. Press Home, put Switch to sleep. 3. Wake after 1 hour. | Campaign resumes instantly. No state loss. Auto-save on suspend (safety net). | P0 |
| **SW-006: Screenshot capture** | System | 1. Greenlight Table screen. 2. Press Capture button. | Screenshot saved to Switch album. No UI glitches or black screens. | P2 |
| **SW-007: File size (cartridge vs. download)** | Storage | 1. Check final build size. | <2GB (physical cartridge 4GB, digital download reasonable). | P1 |

### 10.3 Mobile Tablet Testing (Stretch)

| Test Case | Feature | Steps | Expected Result | Priority |
|-----------|---------|-------|-----------------|----------|
| **TB-001: Touch tap accuracy** | Input | 1. Tap pitch cards, buttons. 2. Rapid tapping. | All taps register. Touch targets ≥44pt (Apple HIG). No missed inputs. | P0 |
| **TB-002: Swipe navigation** | Input | 1. Swipe left/right on Greenlight Table. 2. Navigate between pitches. | Swipe gestures smooth. No accidental taps during swipe. | P1 |
| **TB-003: Pinch-to-zoom (charts)** | Input | 1. Portfolio matrix chart. 2. Pinch to zoom in/out. | Zoom smooth. Max/min zoom limits enforced. Reset button available. | P2 |
| **TB-004: Greenlight stamp (tap-and-hold)** | Input | 1. Tap-and-hold GREENLIGHT stamp (0.5 sec). 2. Release. | Stamp animation plays after hold duration. Prevents accidental greenlights. | P1 |
| **TB-005: Portrait vs. landscape** | Orientation | 1. Rotate tablet. 2. UI adapts. | Landscape preferred (more UI density). Portrait supported but may show warning: "Landscape recommended." | P2 |
| **TB-006: Battery impact** | Performance | 1. Play 1 hour. 2. Monitor battery drain. | <10% battery per hour (low CPU/GPU usage for UI-only game). | P2 |

---

## 11. Regression Testing Checklist

**Run this checklist before each milestone (Alpha, Beta, RC, Launch).**

### Core Systems

- [ ] Greenlight decision executes correctly (budget, trust, morale, project creation)
- [ ] Pass decision executes correctly
- [ ] Counter-offer logic (acceptance/rejection based on trust/desperation)
- [ ] Pitch generation (3-5 pitches, no duplicates, quality seed range valid)
- [ ] Milestone reviews trigger per quarter per active project
- [ ] Interventions modify budget/timeline/morale/quality seed correctly
- [ ] Restraint bonus applies and compounds
- [ ] Projects ship at Gold phase
- [ ] Metacritic resolution (quality seed + morale penalty + variance → final score)
- [ ] Revenue calculation (units sold, price per unit, DLC bonus)
- [ ] Portfolio matrix updates (Prestige/Profit/Populism deltas)
- [ ] Board evaluation (annual review, targets vs. actual, consequences)
- [ ] Studio trust accumulates correctly
- [ ] Studio morale modifies quality seed when < 30
- [ ] Studio archetypes behave per spec (Auteur sensitivity, Reliable tolerance, Volatile variance, Fading decay)
- [ ] Era events trigger on schedule (2003-2012 timeline)
- [ ] Era modifiers apply to genre/platform demand
- [ ] Rumor system telegraphs events 1-2 quarters early

### Save System

- [ ] Manual save creates file
- [ ] Load restores state exactly
- [ ] Auto-save triggers at quarter end
- [ ] Auto-save rotation (max 5 files)
- [ ] Save validation detects corruption
- [ ] Fallback to previous auto-save on corruption
- [ ] Save migration (old saves work in new version)
- [ ] Steam Cloud sync (PC only)

### UI/UX

- [ ] Greenlight stamp animation plays
- [ ] Budget ticker updates smoothly
- [ ] Active projects panel shows all projects
- [ ] Studio relationship panel shows trust/morale
- [ ] Portfolio radar chart updates
- [ ] Board meeting screen displays targets
- [ ] News feed shows era events
- [ ] Tooltips show intervention impacts before applying
- [ ] Loading screens display progress
- [ ] No UI elements clipped or off-screen (all resolutions)

### Platform-Specific

- [ ] PC: Mouse input responsive, keyboard shortcuts work
- [ ] PC: Steam achievements unlock
- [ ] Switch: Controller input (Pro + Joy-Con) works
- [ ] Switch: Handheld mode 30 FPS maintained
- [ ] Switch: Sleep/resume works
- [ ] Tablet: Touch input accurate, tap-and-hold works

### Performance

- [ ] PC: 60 FPS UI, <100ms quarter advance
- [ ] Switch: 30 FPS UI, <200ms quarter advance
- [ ] No memory leaks (5-hour playtest, memory stable)
- [ ] No GC spikes during UI updates
- [ ] Save/load <5 sec worst-case

### Edge Cases

- [ ] Insufficient budget blocks greenlight
- [ ] Project slot limit enforced
- [ ] Low morale (<15) blocks studio pitching
- [ ] Destroyed trust (<-30) removes studio from pool
- [ ] Board fires player after 2 consecutive years satisfaction <50
- [ ] Death spiral scenarios (morale, budget, board) are survivable if player acts
- [ ] Franchise fatigue applies (-3 quality, -5% revenue per installment)
- [ ] Magnum opus unlocks at trust +30 (Auteur only, one-time)

---

## 12. Risk Assessment & Quality Gates

### 12.1 Critical Risks (P0 — Block Launch)

| Risk | Impact | Likelihood | Mitigation | Status |
|------|--------|------------|------------|--------|
| **Save corruption in 10+ hour campaign** | Catastrophic. 1-star reviews. Refunds. | Medium | Atomic file writes, validation, auto-save rotation, extensive QA. | In Progress |
| **Greenlight loop feels arbitrary (not weighty)** | Core loop failure. Game is unplayable. | Low | Playtest Alpha immediately. Iterate on stamp animation, feedback, gut-feel tuning. | In Progress |
| **Formula bugs (quality seed → Metacritic)** | Outcomes feel random. Players lose trust in systems. | Medium | Unit tests on all formulas. Designer tools to preview outcomes. Playtest validation. | In Progress |
| **Unwinnable death spiral (no telegraphing)** | Player feels cheated. Rage quit. | Medium | Board warnings 2 quarters before consequences. Tutorial teaches recovery paths. | Not Started |
| **Switch performance <30 FPS** | Fails platform certification. Cannot ship. | Low | Profile on devkit by Week 8. Reduce UI effects if needed. | Not Started |

### 12.2 High Risks (P1 — Delay Launch)

| Risk | Impact | Mitigation |
|------|--------|------------|
| **Studio AI feels random/unfair** | Players don't understand archetype behaviors. Interventions feel like slot machines. | Tooltips, archetype descriptions, trust/morale graphs. Extensive playtesting. |
| **Mid-game pacing sag (Years 5-7)** | Players drop off. Campaign feels repetitive. | Playtest drop-off tracking. Add scripted mid-game events if needed. |
| **Balance exploits (counter-offer spam, sequel spam)** | Game solvable. Depth collapses. | Unit test economic formulas. Playtest with min-max players. |
| **Platform input inconsistency (mouse vs. controller vs. touch)** | One platform feels second-class. | Abstract input layer. Test all platforms by Week 10. |

### 12.3 Medium Risks (P2 — Post-Launch Patch)

| Risk | Mitigation |
|------|------------|
| **Board targets too harsh/lenient** | Balance tuning via analytics. Patch within 30 days. |
| **Intervention costs unbalanced** | Designer tools + playtest feedback. Tunable via config file (no code change). |
| **Era events feel arbitrary** | Clear rumor system. Historical accuracy in descriptions. |

### 12.4 Quality Gates

**Cannot exit Alpha without:**
- [ ] 30-second greenlight loop feels weighty (playtest validation: 80% of testers hesitate before stamping)
- [ ] Intervention system modifies state correctly (unit tests pass)
- [ ] Save/load functional (no corruption in 20 test saves)
- [ ] No P0 bugs

**Cannot exit Beta without:**
- [ ] Full 3-year campaign completable (5 clean playthroughs)
- [ ] Portfolio matrix comprehensible (playtest: 70% can explain why axes moved)
- [ ] Studio archetypes distinguishable (playtest: testers refer to studios by name, not archetype)
- [ ] No P0 or P1 bugs

**Cannot ship RC without:**
- [ ] Full 10-year campaign completable (5 clean playthroughs)
- [ ] All platforms at target performance (60 FPS PC, 30 FPS Switch)
- [ ] Save corruption rate <0.01% (1000 save/load cycles, 0 failures)
- [ ] Platform certification passed (Switch TCRs, Steam requirements)
- [ ] No P0 bugs, <5 P1 bugs

---

## 13. Test Reporting & Bug Triage

### 13.1 Bug Severity Definitions

| Severity | Definition | Example | SLA |
|----------|------------|---------|-----|
| **P0 (Blocker)** | Prevents core loop, causes crash, corrupts saves. Blocks testing. | Greenlight button doesn't work. Save file unloadable. Game crashes on Year 5. | Fix within 24 hours. |
| **P1 (Critical)** | Major feature broken, exploit, balance disaster. Game shippable but degraded. | Intervention doesn't modify morale. Franchise fatigue not applying. Trust never increases. | Fix within 1 week. |
| **P2 (Major)** | Feature works but incorrectly. Noticeable bug, not blocking. | Tooltip shows wrong value. UI animation stutters. Audio delay. | Fix within 2 weeks or defer to post-launch. |
| **P3 (Minor)** | Cosmetic, rare edge case, doesn't affect gameplay. | Typo in email text. Button alignment off by 2px. | Backlog. Fix if time permits. |

### 13.2 Bug Report Template

**Title**: [Component] Short description
**Severity**: P0 / P1 / P2 / P3
**Platform**: PC / Switch / Tablet
**Build**: Version number
**Repro Rate**: 100% / 50% / 10% / Once

**Steps to Reproduce**:
1. Step one
2. Step two
3. Step three

**Expected Result**: What should happen.
**Actual Result**: What actually happens.
**Attachments**: Screenshots, save files, logs.
**Notes**: Additional context.

### 13.3 Triage Process

**Daily standup**: QA lead reviews new bugs. Assigns severity. Routes to engineer.
**Weekly review**: Product owner + lead engineer + QA lead. Re-prioritize P1/P2 bugs. Decide what ships vs. defers.
**Pre-milestone**: Full bug review. No P0s in build. P1 count tracked. RC must have <5 P1s.

---

## 14. Playtesting Focus Areas

### 14.1 Alpha Playtest (Weeks 4-6)

**Focus**: Core loop validation. Does the 30-second decision feel meaningful?

**Test Protocol**:
- 10 external testers (mix of management sim veterans + new players)
- 1-hour session: Years 2003-2004 (8 quarters)
- Watch testers play (no instructions beyond tutorial)
- Post-session interview:
  - "Did you hesitate before greenlighting? Why?"
  - "Did interventions feel strategic or arbitrary?"
  - "Did you understand the gut-feel indicator?"
  - "Which studio do you remember? Why?"

**Success Criteria**:
- 80% of testers hesitate before stamping greenlight (decision feels weighty)
- 70% can explain one intervention trade-off without prompting
- 60% remember at least one studio by name

### 14.2 Beta Playtest (Weeks 10-12)

**Focus**: Full systems integration. Does the 3-year arc hold?

**Test Protocol**:
- 15 testers (same pool + 5 new)
- 3-hour session: Years 2003-2005 (12 quarters)
- Track: session drop-off (do testers complete 3 hours?)
- Post-session survey:
  - "Can you explain why your Prestige/Profit/Populism changed?"
  - "Did you anticipate the Xbox 360 launch (rumor system)?"
  - "Which studio do you trust most? Why?"

**Success Criteria**:
- 80% session completion rate (testers don't quit mid-session)
- 70% can explain portfolio matrix movements
- 60% successfully positioned for era event (Wii/Xbox 360)

### 14.3 RC Playtest (Weeks 18-20)

**Focus**: Full campaign pacing. Does the 10-year arc hold?

**Test Protocol**:
- 20 testers
- Full campaign: 2003-2012 (10 years, 12-18 hours across multiple sessions)
- Track: drop-off points (which year do players quit?)
- Track: bugs encountered, crashes, save corruption
- Post-campaign interview:
  - "What was your publisher's identity (Prestige/Profit/Populism)?"
  - "Name a studio that mattered to you. Why?"
  - "Was the ending satisfying?"

**Success Criteria**:
- 80% campaign completion rate (testers reach Year 10)
- <30% drop-off in Years 5-7 (mid-game pacing validated)
- 90% can articulate their publisher's identity
- 0 save corruption incidents across 20 full campaigns

---

## 15. Launch Readiness Checklist

**This checklist must be 100% complete before launch.**

### Code & Build

- [ ] All P0 bugs fixed
- [ ] <5 P1 bugs remaining (documented, known shippable)
- [ ] All platforms build without errors
- [ ] Final build smoke test (boot → full quarter → save → quit → load)
- [ ] Version number finalized (1.0.0)
- [ ] Credits screen complete

### Content

- [ ] 50+ pitch concepts authored
- [ ] 15 studio definitions finalized
- [ ] All era events authored (2003-2012)
- [ ] All intervention descriptions finalized
- [ ] Tutorial text finalized
- [ ] All emails/narrative beats proofread (no typos)

### Platform Certification

- [ ] PC: Steam build uploaded to Steam partner portal
- [ ] PC: Steam Cloud saves tested
- [ ] PC: Achievements tested (all 30-40 unlock correctly)
- [ ] Switch: Lotcheck submission passed (Nintendo certification)
- [ ] Switch: TCRs validated (sleep/resume, screenshot, button prompts)
- [ ] Switch: eShop listing approved

### Save System

- [ ] 1000 save/load cycles: 0 corruption failures
- [ ] Auto-save rotation tested (5 files max)
- [ ] Save migration tested (v1.0 → v1.1 hypothetical)
- [ ] Steam Cloud sync tested on 3 different PCs

### Performance

- [ ] PC: 60 FPS UI, <100ms quarter advance (verified on min-spec PC)
- [ ] Switch: 30 FPS UI, <200ms quarter advance (handheld mode)
- [ ] Memory leak test: 5-hour playthrough, memory stable
- [ ] Load time: <5 sec worst-case (Year 10 save)

### QA Sign-Off

- [ ] Full campaign playthrough: 5 clean runs (no crashes, no blocking bugs)
- [ ] Regression checklist 100% passed
- [ ] All quality gates met (Alpha/Beta/RC)
- [ ] Final playtest: 20 testers, 80% completion rate, positive sentiment

### Marketing & Support

- [ ] Launch trailer captured and rendered
- [ ] Steam page live (capsule art, screenshots, description)
- [ ] Press builds distributed
- [ ] Day-one patch plan documented (if needed)
- [ ] Support FAQ written (common issues, save file location, bug reporting)

**QA Lead Sign-Off**: ___________________
**Producer Sign-Off**: ___________________
**Date**: ___________________

---

## 16. Post-Launch Support Plan

### Month 1: Bug Triage & Hotfix

**Focus**: Monitor player reports. Triage crashes and save corruption immediately.

**SLA**:
- P0 (crash, save corruption): Hotfix within 48 hours
- P1 (major bug): Patch within 2 weeks
- Balance feedback: Collect data, analyze, plan Patch 1.1

**Monitoring**:
- Steam reviews (scan for bug mentions)
- Support email / Discord
- Reddit / Twitter mentions
- Analytics: session length, drop-off points, average campaign completion time

### Month 2: Patch 1.1 (Balance & QoL)

**Deliverables**:
- Balance tuning (board targets, intervention costs) based on analytics
- QoL features: Fast-forward quarter button, undo last decision
- Bug fixes (all P1s from launch backlog)

### Month 4: Patch 1.2 (Content Expansion)

**Deliverables**:
- 2-3 new studio archetypes
- 5 new interventions
- Expanded random event pool
- Additional balance tuning

### Month 8: Expansion DLC (Stretch)

**Scope**:
- "The Indie Boom" expansion
- Extend campaign 2013-2015 (3 years, 12 quarters)
- Digital distribution dominance era
- Crowdfunding mechanics
- Early Access greenlighting

---

## Closing Notes

This QA plan is the contract between what we promise and what we deliver. Every test case is a question we must answer before the player asks it. Every edge case is a failure mode we must find before it finds us.

The game is about decisions made with incomplete information and permanent consequences. So is QA. We test what we can. We anticipate what we can't. And when we miss something — and we will miss something, because we always do — we triage it, fix it, and learn from it.

**What does "works" mean for GREENLIGHT?**

- The greenlight stamp feels weighty every single time.
- Interventions feel strategic, not arbitrary.
- Formulas are fair, not random.
- Studio relationships feel like people, not stat blocks.
- The 10-year campaign holds without sagging.
- Saves never corrupt.
- The player hesitates before pressing the button.

If we ship that, we've done our job. If we ship anything less, we haven't.

The seat is still warm. The folder is on the desk. The studio is waiting.

Define "works."

---

**Document Status**: Production-ready. Approved for QA execution.

**Next Steps**: Week 1 kickoff. Alpha playtest prep. Greenlight Table validation or bust.

— CRASH
