# GREENLIGHT -- Game Design Document

**Designer**: REED
**Version**: 1.0
**Date**: 2026-02-07
**Status**: Pre-Production

---

> "A game is its loop. If the 30-second experience isn't compelling, no amount of content saves it."
> -- REED

---

## 1. Player Fantasy Statement

**You are the most powerful person in gaming who never touches a controller -- the publisher executive whose signature decides which dreams live, which die, and which become legends -- and every greenlight you stamp is a bet you'll be living with for years.**

That's the fantasy. Not creation. Not authorship. *Patronage.* The player wants to feel the weight of the gatekeeper's chair -- the intoxicating, isolating, morally complicated power of choosing whose vision gets funded. The emotional hook is not "I made something great." It's "I *chose* something great, and I was right -- or I was wrong, and I have to sit in this chair and stare at the consequences."

What's the player's verb? **Decide.** Not build, not optimize, not manage (though management is the medium). The atomic unit of this game is a decision made with incomplete information and permanent consequences. Every system feeds that verb. Every mechanic exists to make the next decision harder, more informed, and more personal than the last.

Where's the depth? The depth is that the same verb -- greenlight or pass -- transforms completely over 20 hours. At hour 1, you're evaluating a pitch on its surface. At hour 20, you're evaluating a pitch in the context of a studio relationship you've cultivated across three years, an era shift you anticipated six months ago, a portfolio imbalance the board is breathing down your neck about, and a rival publisher who just poached the studio you were saving for Q3. Same stamp. Completely different decision.

---

## 2. Core Loop

### The 30-Second Loop: The Greenlight Decision

```
    +-------------------+
    |   PITCH ARRIVES   |
    | (Studio + Concept |
    |  + Market Context)|
    +--------+----------+
             |
             v
    +-------------------+
    |     EVALUATE      |
    | Read dossier.     |
    | Check studio rep. |
    | Read gut-feel.    |
    | Weigh portfolio.  |
    +--------+----------+
             |
             v
    +-------------------+
    |      DECIDE       |
    | [GREENLIGHT]      |
    | [PASS]            |
    | [COUNTER-OFFER]   |
    +--------+----------+
             |
             v
    +-------------------+
    | IMMEDIATE FEEDBACK|
    | Studio reacts.    |
    | Budget shifts.    |
    | Slot fills.       |
    | Tension resolves. |
    +-------------------+
```

This loop must take 15-30 seconds per pitch. The player reads, weighs, decides. The feel is a poker hand: you have partial information, a gut read, and a finite stack of chips. The stamp comes down. The studio reacts. You feel it. Move to the next pitch or end the quarter.

### The 5-Minute Loop: The Development Cycle

```
    +-------------------+
    |  GREENLIGHT TABLE |
    |  (Quarterly)      |
    |  3-5 pitches,     |
    |  choose 1-2       |
    +--------+----------+
             |
             v
    +-------------------+
    |  MILESTONE REVIEW |
    |  Active projects  |
    |  report status:   |
    |  Build / Morale / |
    |  Budget / Risk    |
    +--------+----------+
             |
             v
    +-------------------+
    |   INTERVENTION    |
    |  OR RESTRAINT     |
    |  (per project)    |
    +--------+----------+
             |
             v
    +-------------------+
    |   ERA EVENTS      |
    |  Industry shifts. |
    |  Console launches.|
    |  Market disrupts. |
    +--------+----------+
             |
             v
    +-------------------+
    |  RESULTS PHASE    |
    |  Games ship.      |
    |  Metacritic hits. |
    |  Revenue tallied. |
    |  Studios react.   |
    +--------+----------+
             |
             +---------> (back to GREENLIGHT TABLE)
```

Each quarter takes 3-5 minutes. The player cycles through the greenlight phase (new pitches), the management phase (milestone reviews on active projects), the event phase (industry changes), and the results phase (shipped games, scores, revenue). This is the session rhythm -- the "one more quarter" cadence that drives play sessions.

### The Meta Loop: The Publisher's Legacy

```
    +------------------------------------------------------+
    |                    FISCAL YEAR                        |
    |                                                      |
    |  Q1: Greenlight + Milestones + Era Event             |
    |  Q2: Greenlight + Milestones + Era Event             |
    |  Q3: Greenlight + Milestones + E3 Showcase           |
    |  Q4: Greenlight + Milestones + Holiday Ship Window   |
    |                                                      |
    +---------------------------+--------------------------+
                                |
                                v
    +------------------------------------------------------+
    |                  ANNUAL REVIEW                        |
    |                                                      |
    |  Metacritic Roundup (all shipped titles)              |
    |  Revenue Report (P&L per title)                       |
    |  Portfolio Matrix Shift (Prestige/Profit/Populism)    |
    |  Board Evaluation (targets met? warnings issued?)     |
    |  Studio Relationship Update (trust/grudge shifts)     |
    |  Reputation Change (what caliber studios pitch you?)  |
    |                                                      |
    +---------------------------+--------------------------+
                                |
                                v
    +------------------------------------------------------+
    |                  NEW YEAR BEGINS                      |
    |                                                      |
    |  Era Engine advances (new industry conditions)        |
    |  New studios enter the pitch pool                     |
    |  Old studios evolve (morale, trust, capability)       |
    |  Board sets new targets                               |
    |  Player's portfolio reputation opens/closes doors     |
    |                                                      |
    +------------------------------------------------------+
```

The full campaign spans 2003-2012 (10 fiscal years, 40 quarters). A complete playthrough runs 12-18 hours. The meta loop is about legacy accumulation: each year's decisions compound into a publisher identity that determines which studios want to work with you, what the board expects, and how the industry perceives you. This is where the "hour 20" depth lives -- the player is no longer making isolated bets but orchestrating a decade-long strategy across relationships, market shifts, and portfolio identity.

---

## 3. Core Mechanics

### 3.1 The Greenlight Table

**Player Verb**: Evaluate and decide.

**System Description**: Each quarter, the system generates 3-5 pitches drawn from the active studio pool. Each pitch is a structured dossier containing:

| Dossier Field | Source | Visibility |
|---|---|---|
| Studio Name & Archetype | Studio entity | Full |
| Studio Trust Level (with you) | Relationship system | Full |
| Studio Morale | Studio entity | Full |
| Game Concept (genre, platform, hook) | Authored pitch + procedural modifiers | Full |
| Projected Budget | System calculation | Full |
| Estimated Timeline (quarters) | System calculation | Full |
| Risk Factors (0-3 flags) | System analysis | Full |
| Market Fit Score | Era Engine cross-reference | Partial (shown as Low/Med/High) |
| Gut-Feel Indicator | Player's Executive Intuition stat | Partial (accuracy scales with stat) |
| Hidden Quality Seed | RNG + studio capability + concept fit | Hidden |

The Hidden Quality Seed is the critical variable. It's calculated as:

```
Quality_Seed = (Studio_Capability * 0.35) + (Concept_Fit_to_Era * 0.25) +
               (Studio_Morale * 0.15) + (Random_Variance * 0.25)
```

The player never sees this number. The Gut-Feel Indicator is a noisy signal derived from it:

```
Gut_Feel_Display = Quality_Seed + (Random(-15, +15) / Executive_Intuition_Level)
```

At Intuition Level 1 (starting), the noise range is +/-15 points on a 100-point scale. At Intuition Level 5 (late game), it narrows to +/-3. The player learns to trust their gut more as they gain experience, but it's never perfectly accurate. This is essential -- if the player can solve the greenlight decision mathematically, the tension dies.

**Decision Options**:

| Decision | Effect |
|---|---|
| **Greenlight** | Full budget approved. Studio morale +10. Fills one active project slot. Relationship trust +5. |
| **Pass** | No budget impact. Studio morale -5. Relationship trust -3. Studio may pitch to rival publisher. |
| **Counter-Offer: Reduced Budget** | Budget cut by 20-40% (player chooses). Timeline unchanged. Studio morale -5 to -15 (scales with cut severity). Quality_Seed penalized by (cut_percentage * 0.5). Studio may accept or reject (acceptance chance = Trust_Level * 0.7 + Desperation * 0.3). |
| **Counter-Offer: Conditional Greenlight** | Full budget, but with an attached condition (e.g., "Must include multiplayer," "Must hit Holiday ship date," "Must target Platform X"). Studio morale -5 to -20 depending on condition severity vs. archetype sensitivity. Quality_Seed modified by condition-specific formula. |

**Feedback**: When the player stamps GREENLIGHT, the stamp animation plays with a satisfying mechanical THUNK. The studio representative's portrait shifts -- a smile, a nod, a fist pump depending on archetype. The budget ticker on the sidebar decrements. The active projects panel gains a new card. For PASS, the stamp is a red X, the studio rep's expression falls, and the pitch slides off-screen. For counter-offers, a negotiation beat plays -- the studio considers, then accepts or rejects with a visible emotional reaction.

**Depth**: At hour 1, the player greenights things that sound cool. By hour 10, they're reading the dossier against the Era Engine forecast, checking if they have genre coverage gaps in their portfolio, weighing whether this studio's archetype fits their intervention style, and considering whether passing on this pitch will push the studio toward a rival publisher who'll use them to compete in the same holiday window. The decision is the same. The decision space is transformed.

### 3.2 The Intervention System

**Player Verb**: Intervene or restrain.

**System Description**: At each milestone review (one per quarter per active project), the player receives a status package and may choose one intervention or choose to do nothing. Restraint is always an option and is sometimes the optimal choice -- particularly with Auteur-archetype studios.

**Intervention Table** (15 core interventions):

| Intervention | Budget Impact | Timeline Impact | Morale Impact (Base) | Quality Modifier | Archetype Sensitivity |
|---|---|---|---|---|---|
| Force Multiplayer | +$8M | +2 quarters | -15 | +5 to +15 (genre-dependent) | Auteur: -25 morale. Reliable: -5 morale. |
| Demand Sequel Hook | +$2M | +0 | -10 | -5 (dilutes focus) | Auteur: -20. Volatile: -15. |
| Cut Open World | -$12M | -2 quarters | -20 | -10 to +5 (depends on execution risk) | Auteur: -30. Fading: -10. |
| Ship It Now | $0 | Set to 0 (ships next quarter) | -25 | Quality_Seed * 0.6 (locks current state) | All archetypes: heavy morale hit. |
| Expand Scope | +$15M | +3 quarters | +15 | +10 to +25 (high variance) | Auteur: +20 morale. Reliable: -10 morale. |
| Delay to Polish | +$5M | +1 quarter | +5 | +8 (diminishing per repeat) | All: generally positive. |
| Change Art Direction | +$6M | +2 quarters | -20 | -5 to +15 (high variance) | Auteur: -35. Volatile: -10. |
| Increase Marketing | +$10M | +0 | +5 | +0 (doesn't change quality) | All: minor positive morale. |
| Mandate DLC Plan | +$3M | +0 | -10 | -3 (splits dev focus) | Auteur: -20. Reliable: +5. |
| Attach Licensed IP | +$8M (licensing fee) | +1 quarter | -10 | -5 to +10 (IP fit dependent) | Volatile: -20. Fading: +10. |
| Fire Lead Designer | +$2M (severance) | +1 quarter (reorganization) | -30 | -15 to +15 (replacement lottery) | Auteur: relationship destroyed. |
| Reduce to Handheld | -$10M | -1 quarter | -15 | -10 (scope loss) | Auteur: -25. Reliable: -5. |
| Port to Next-Gen | +$7M | +1 quarter | +0 | +5 (tech upgrade) | Reliable: +5 morale. Fading: -10. |
| Cancel Project | Recovers 20% remaining budget | Frees slot immediately | -40 | N/A | All: devastating. Trust -20 to -40. |
| Greenlight Expansion | +$5M | +2 quarters (post-ship) | +10 | N/A (applies to shipped title) | All: generally positive. |

**Restraint Bonus**: If the player does not intervene on a milestone, the studio receives a passive +3 morale bonus ("They trust us"). For Auteur studios, this bonus is +8. Consecutive restraint across multiple milestones compounds: +3, +5, +8, +12. This rewards players who learn to identify which studios thrive with autonomy. However, restraint on a project in crisis (budget overrun > 30% or morale < 25) is flagged by the board as negligence and costs -5 Board Confidence.

**Feedback**: Each intervention triggers an immediate visible reaction -- the project card updates with new budget/timeline numbers, the studio morale bar shifts with an animated fill/drain, and a brief narrative beat appears (e.g., "Studio Vanguard's lead designer sent a terse email: 'We'll add multiplayer. But this isn't what we pitched.'"). The quality modifier is hidden until the game ships and reviews arrive.

**Depth**: A novice player intervenes reactively -- they see a problem, they apply a fix. An expert player intervenes strategically -- they force multiplayer on a Reliable studio's action game in Q1 2005 because they know Xbox Live adoption is about to spike, and the +15 quality bonus for "multiplayer in an online-ready era" will push the Metacritic score above 85. They also know this studio won't resent it because Reliable archetypes tolerate structural mandates. That's systems literacy creating informed intervention.

### 3.3 The Portfolio Matrix

**Player Verb**: Balance and prioritize.

**System Description**: The publisher's identity is tracked across three axes, each scored 0-100:

- **Prestige** (P1): Critical acclaim, award wins, developer reputation. Fueled by high-Metacritic titles, respected studios, and creative risk.
- **Profit** (P2): Revenue, market share, shareholder return. Fueled by high-selling titles, franchise exploitation, and cost efficiency.
- **Populism** (P3): Mainstream appeal, install base reach, cultural penetration. Fueled by accessible games, broad platform coverage, and marketing spend.

Each shipped game modifies all three axes:

```
Prestige_Delta  = (Metacritic_Score - 70) * 0.4 + Studio_Prestige_Rating * 0.3 + Genre_Innovation_Flag * 10
Profit_Delta    = (Revenue - Budget) / Budget * 15 + Franchise_Flag * 5
Populism_Delta  = Units_Sold / 500000 * 10 + Platform_Reach_Multiplier * 5 + Marketing_Spend_Flag * 3
```

**Board Targets**: Each fiscal year, the board sets minimum thresholds for each axis. These start lenient (Year 1: P1 > 30, P2 > 30, P3 > 30) and tighten as the campaign progresses. By Year 7, the board demands specialization -- you can't be mediocre at everything.

**Board Target Escalation Table**:

| Year | Prestige Min | Profit Min | Populism Min | Special Condition |
|---|---|---|---|---|
| 1 (2003) | 30 | 30 | 30 | None (grace period) |
| 2 (2004) | 35 | 35 | 35 | None |
| 3 (2005) | 40 | 40 | 35 | Next-gen pivot expected |
| 4 (2006) | 40 | 45 | 40 | Casual market response expected |
| 5 (2007) | 45 | 50 | 45 | Portfolio diversification expected |
| 6 (2008) | 45 | 55 | 45 | DLC revenue stream expected |
| 7 (2009) | 50 | 55 | 50 | At least one axis above 70 |
| 8 (2010) | 50 | 60 | 50 | At least one axis above 75 |
| 9 (2011) | 55 | 60 | 55 | Two axes above 65 |
| 10 (2012) | 55 | 65 | 55 | Board demands clear identity |

**Consequence Table for Axis Failures**:

| Axis Below Minimum | Warning (1st time) | Consequence (2nd consecutive) | Crisis (3rd consecutive) |
|---|---|---|---|
| Prestige | "Developers are talking. Our reputation is slipping." Top-tier studios reduce pitch frequency by 25%. | Auteur and Volatile studios stop pitching entirely. Award show invitations revoked. | Board forces you to acquire a prestige studio at 2x market rate, eating $30M of budget. |
| Profit | "Shareholders are concerned." Annual budget reduced by 10%. | Budget reduced by 25%. One active project slot removed. | Board installs a CFO who vetoes any project with projected ROI below 150%. |
| Populism | "We're losing shelf space." Retail visibility drops. Marketing costs increase 15%. | Platform holders deprioritize your titles. Launch window access restricted. | Board demands a "mass market initiative" -- you must greenlight at least 2 games targeting casual audiences per year. |

**Feedback**: The Portfolio Matrix is displayed as a persistent triangle radar chart in the HUD. Each axis pulses gently toward green when above target and toward red when below. After each shipped game, the triangle visibly reshapes with a satisfying animation. The board's annual evaluation is presented as a formal boardroom scene with named board members commenting on each axis.

**Depth**: The tension is genuine. A critically acclaimed indie that scores 94 on Metacritic and sells 200K copies is a Prestige rocket and a Profit anchor. A licensed movie tie-in that scores 62 and sells 5 million copies is Profit and Populism gold but Prestige poison. The player cannot maximize all three and must decide what kind of publisher they are. This identity then determines which studios want to work with them, creating a self-reinforcing strategic loop.

### 3.4 The Studio Relationship Web

**Player Verb**: Cultivate and navigate.

**System Description**: Each of the 12-15 studios is a persistent entity with the following attributes:

| Attribute | Range | Description |
|---|---|---|
| Archetype | Auteur / Reliable / Volatile / Fading | Core personality. Determines intervention sensitivity, pitch style, and narrative arc. |
| Capability | 1-100 | Raw development quality. Auteurs start high (70-85). Fading studios start high but decay (80 -> 50 over campaign). |
| Morale | 0-100 | Current emotional state. Affects quality output, pitch willingness, and intervention tolerance. Below 20 = crisis risk. |
| Trust (toward player) | -50 to +50 | Accumulated relationship score. Positive trust unlocks exclusive pitches and tolerates interventions. Negative trust reduces pitch quality and risks studio departure. |
| Industry Reputation | 0-100 | How the broader industry perceives this studio. Affects your Prestige when you publish their work. |
| Desperation | 0-100 | Financial/career pressure. High desperation studios accept worse counter-offers but produce lower-quality work under pressure. |

**Archetype Behavior Profiles**:

**Auteur Studios** (3-4 in pool): High Capability (75-90), high morale volatility, extreme intervention sensitivity. They pitch ambitious, over-scoped games with high Quality_Seed ceilings but wide variance. Trust is earned through restraint -- every intervention costs double morale. But if Trust exceeds +30, they offer their "magnum opus" pitch: a high-risk, high-reward project that can score 90+ and define the publisher's legacy. If Trust drops below -20, they leave permanently and publicly criticize you in industry press (-10 Prestige).

**Reliable Studios** (4-5 in pool): Moderate Capability (60-75), stable morale, low intervention sensitivity. They pitch sequels, franchise entries, and genre staples with narrow Quality_Seed ranges (predictable 70-82 Metacritic). They tolerate interventions well and are the backbone of Profit. They never produce a masterpiece but rarely produce a disaster. If Trust exceeds +25, they offer franchise expansion opportunities (spinoffs, handhelds) at reduced budget.

**Volatile Studios** (3-4 in pool): Variable Capability (40-90, shifts per project), extreme morale swings, moderate intervention sensitivity. They pitch wild concepts -- genre mashups, experimental mechanics, niche audiences. Quality_Seed variance is maximum: they'll produce a 92 or a 45 with roughly equal probability. Morale swings of +/-20 per quarter are normal. Gentle support (low-intervention, morale-positive actions) stabilizes them. Micromanagement destabilizes them. If a Volatile studio ships a hit (85+ Metacritic), their Capability permanently increases by 10 -- they've found their voice. If they ship a disaster (below 50), Desperation spikes and they may accept predatory counter-offers from the player or rival publishers.

**Fading Studios** (2-3 in pool): Declining Capability (starts 75-85, loses 5 per year), moderate morale (starts 50, declines with failures), low intervention sensitivity but high emotional fragility. They pitch sequels to their classic franchises and "return to form" projects. Their Quality_Seed formula has an extra term: `+ Nostalgia_Bonus * Era_Match` -- their games sell better in early eras when their brand still resonates. A Fading studio can be reinvented if the player greenlights a new-IP project (not a sequel) and it scores above 80 -- their Capability decay halts and reverses by 3 per year. Otherwise, they eventually stop pitching and retire in a narrative event.

**Inter-Studio Communication**: Studios share information. When you cancel a project, all studios with Trust < +10 lose 3 Trust ("If they did it to them, they'll do it to us"). When you ship a critically acclaimed game, studios with the same archetype gain +5 Trust ("They understand people like us"). When you fire a lead designer, all Auteur studios lose 8 Trust regardless of relationship. This creates a reputation economy that operates alongside the financial one.

**Feedback**: Each studio has a portrait with subtle expression changes reflecting morale and trust. Their pitch emails are written in distinct voices -- the Auteur's emails are long, passionate, slightly pretentious; the Reliable's are professional templates; the Volatile's are erratic, sometimes brilliant, sometimes barely coherent; the Fading studio's are nostalgic, referencing past glories. When trust changes, a brief relationship-update notification appears: "Vanguard Interactive: Trust +5. They remember that you believed in Project Aurora."

**Depth**: The relationship web is the long game. An expert player thinks in terms of studio arcs -- "I'm going to keep nurturing Eclipse Games (Auteur) for three years with hands-off publishing so they offer me their magnum opus in 2007, right when the next-gen install base hits critical mass." This requires sacrifice: the Auteur's early games might underperform commercially, dragging Profit. But the payoff is a generation-defining title that transforms the publisher's Prestige permanently. This kind of multi-year investment is where Greenlight transcends quarterly optimization and becomes legacy management.

### 3.5 The Era Engine

**Player Verb**: Anticipate and adapt.

**System Description**: Each in-game year triggers 2-4 industry events drawn from a historically-grounded event table. Events modify the simulation's underlying parameters.

**Era Event Table (Core Events)**:

| Year | Event | Systemic Effect |
|---|---|---|
| 2003 | Online console gaming emerges (Xbox Live year 1) | Games with multiplayer: +5 Quality_Seed. Platform: Xbox gains +10 install base growth rate. |
| 2004 | Sequelitis peaks; original IP risk at all-time high | Sequel games: +8 Profit_Delta. New IP: -5 Profit_Delta (but +5 Prestige if scored above 80). |
| 2004 | Handheld renaissance (DS + PSP announced) | Handheld platform slot opens. Handheld budgets: 40% of console. Handheld market demand: +15 per year through 2007. |
| 2005 | Xbox 360 launches (next-gen begins) | Next-gen projects: +10 Quality_Seed if polished, -15 if rushed. Development costs: +30% for next-gen. PS2 install base still massive but revenue-per-unit declining. |
| 2005 | Metacritic influence grows | Review scores now affect revenue more strongly: Revenue_Multiplier += (Metacritic - 70) * 0.02. Board begins tracking average Metacritic. |
| 2006 | PS3 launches at $599 (troubled launch) | PS3-exclusive projects: high risk, high Prestige if successful. PS3 install base growth: slow (25% of Xbox 360 rate for Year 1). |
| 2006 | Wii launches (casual disruption) | Casual/party games: +20 Populism_Delta. Traditional core games on Wii: -10 Quality_Seed (audience mismatch). Wii install base: explosive (3x Xbox 360 growth rate). Board demands "casual strategy response." |
| 2007 | Digital distribution emerges (Steam grows, XBLA matures) | Digital-only budget games become viable: Budget cap $5M, no retail costs, 70% margin. Indie/Volatile studios can pitch digital-only projects. |
| 2007 | Prestige game era (BioShock, Portal, Mass Effect year) | Original IP with strong creative vision: +10 Prestige_Delta. Industry-wide Prestige expectations rise. Board minimum Prestige increases by +5. |
| 2008 | DLC economy emerges | All shipped games can now generate DLC revenue: +15-30% post-launch revenue if DLC mandated during development. Studios with "Mandate DLC Plan" intervention: -5 morale but +$3-8M lifetime revenue per title. |
| 2008 | Global recession | Total annual budget reduced by 15%. Board Profit expectations temporarily frozen (no increase this year). Studios accept lower counter-offers (Desperation industry-wide +15). |
| 2009 | Mobile gaming begins ascent | Board asks "What's our mobile strategy?" Ignoring mobile: -5 Populism per year going forward. Investing in mobile: opens new low-budget project slot but cannibalizes handheld revenue. |
| 2010 | Motion control wars (Kinect, Move) | Motion-control projects: high Populism, moderate risk. Reliable studios can pivot to motion-control at low morale cost. Auteur studios refuse. |
| 2011 | Live service model emerges | Games with online components can generate recurring revenue. Live-service mandate: +$5M budget, +2 quarters, but +40% lifetime revenue. Morale impact varies wildly by archetype. |
| 2012 | Industry consolidation | Studio acquisition opportunities appear. Fading studios can be bought (cost: 2x annual revenue). Acquired studios become "internal" -- no trust mechanics, but reduced creativity. Campaign finale: legacy evaluation. |

**Random Events** (2-3 per year, drawn from a pool of 40+): Studio lead quits mid-project. Competitor announces a game in your niche. Platform holder offers exclusivity deal ($5M bonus, locks to one platform). Focus group report arrives (noisy signal about market demand). Industry award nominations (Prestige boost if you win). Studio scandal (morale crisis, media attention). Surprise hit in adjacent genre (shifts market demand curves).

**Feedback**: Era events are presented as in-game "news feeds" styled as 2005-era gaming websites -- think IGN circa 2006, with headlines, comment sections, and analyst quotes. Major events trigger a full-screen "Industry Shift" notification with visible parameter changes: "The Wii launches. Casual market demand: UP. Core gamer platform loyalty: DISRUPTED. Your move."

**Depth**: The Era Engine is a strategic forecasting system. The player who reads history (or develops a sense for the simulation's patterns on repeat playthroughs) can position their portfolio ahead of shifts. Greenlighting a casual party game in Q2 2005 for a Q4 2006 ship window means it launches alongside the Wii -- that's a 12-18 month strategic bet that pays enormous dividends. But it also means sacrificing a greenlight slot that could have gone to a "safe" sequel. The Era Engine transforms the Greenlight Table from a series of isolated bets into a forward-looking strategy game.

---

## 4. Systems Interactions

### Positive Feedback Loops (Snowball Effects)

**The Prestige Flywheel**: Ship a high-Metacritic game -> Prestige rises -> Higher-quality studios pitch to you -> Better pitches available -> Higher chance of shipping another hit -> Prestige rises further. This loop is powerful and intentional -- it rewards players who invest in quality. But it's gated by Profit: prestige games often don't make money, and a Profit crash cuts your budget before the flywheel completes.

**The Trust Compound**: Treat a studio well -> Trust rises -> Studio pitches better concepts (Quality_Seed bonus of Trust * 0.3) -> Better games ship -> Trust rises further -> Studio offers exclusive "magnum opus" pitch. This loop takes 3-5 years to mature and is the primary long-game reward. It's the deepest strategic investment in the game.

**The Franchise Snowball**: Ship a successful game -> Sequel opportunity at reduced budget (80% of original) and reduced risk (Quality_Seed floor raised by 10) -> Sequel ships, generates reliable Profit -> Third installment greenlit with franchise marketing bonus -> Revenue compounds. This loop is the Profit engine. It's tempting and reliable -- which is why it must be counterbalanced.

### Negative Feedback Loops (Rubber Banding)

**The Sequel Fatigue Curve**: Each successive franchise installment gets a cumulative -3 Quality_Seed and -5% revenue (audience fatigue). By the 4th installment, the franchise is coasting on brand alone with declining returns. This prevents infinite franchise exploitation and forces the player to invest in new IP periodically.

**The Overextension Trap**: Greenlighting too many projects simultaneously splits management attention. With 5+ active projects, each milestone review is time-pressured (the player has less real time to evaluate each one carefully). Additionally, budget strain increases intervention costs by 20% when utilization exceeds 80% of annual budget. This rubber-bands aggressive expansion.

**The Morale Death Spiral**: When studio morale drops below 30, Quality_Seed is penalized by -(30 - Morale) * 0.5. A studio at morale 10 loses 10 points of Quality_Seed. This often leads to a poor game, which further tanks morale. The escape from the spiral is either a no-intervention quarter (passive morale recovery of +5/quarter) or a costly Expand Scope / Delay to Polish intervention. Players must learn to read the spiral before it starts.

**The Board Pressure Ratchet**: As board targets increase yearly, maintaining the status quo becomes insufficient. A publisher that was thriving in Year 3 may be failing by Year 6 without growth. This creates escalating pressure that prevents comfortable plateau states and forces ongoing strategic adaptation.

### Emergent Possibilities

**The Calculated Sacrifice**: A player greenlights a game they know will underperform commercially but will score 90+ on Metacritic, deliberately eating a Profit loss to spike Prestige before a critical board evaluation that demands higher Prestige. This is portfolio manipulation -- using one game as a strategic instrument rather than an end in itself. The game doesn't teach this. Players discover it through systems literacy.

**The Studio Rescue Arc**: A Fading studio with declining Capability pitches a desperate reboot of their classic franchise. A naive player passes. An expert player sees the opportunity: greenlight at full budget, attach no conditions, let the nostalgia factor carry marketing, and use the "Delay to Polish" intervention to buy extra quality. If the reboot scores above 80, the studio's Capability decay reverses. The player has just saved a studio's legacy -- and earned an ally for life (Trust +25). This emergent narrative is not scripted. It arises from the intersection of studio attributes, intervention rules, and quality calculations.

**The Counter-Cycle Play**: When the 2008 recession hits and budgets contract, most publishers pull back. A player with reserves can counter-cycle: greenlight ambitious projects at reduced rates (desperate studios accept 70% budget) and position them to ship in 2010-2011 when the market recovers. This mirrors real-world contrarian investment strategy and emerges naturally from the Era Engine's economic modifiers.

**The Reputation Cascade**: Cancel a game cruelly (low morale studio, public cancellation). All studios lose Trust. Auteur studios stop pitching. Prestige drops. Only Reliable studios remain in the pitch pool. The player is trapped publishing safe sequels, generating Profit but bleeding Prestige. The board demands Prestige. The player has no prestige-capable studios left. This death spiral is emergent and devastating -- and completely avoidable with careful relationship management.

---

## 5. Progression Design

### Player Growth Axes

**Knowledge Growth** (invisible, player-skill-based): The player learns to read the systems -- which archetypes respond to which interventions, how the Era Engine's trends interact with genre demand, when to intervene and when to hold. This is the primary progression axis and it's purely skill-based. No unlock gates it. A knowledgeable player on a fresh save is more effective than a clueless player at Year 5.

**Executive Intuition** (visible stat, 1-5): Improves the accuracy of the Gut-Feel Indicator on pitch dossiers. Levels up based on correct predictions (greenlighting games that score within 10 points of the gut-feel estimate, or passing on games that would have scored below 60).

| Intuition Level | Gut-Feel Noise Range | Unlock Trigger |
|---|---|---|
| 1 | +/-15 | Starting value |
| 2 | +/-12 | 5 correct predictions |
| 3 | +/-9 | 12 correct predictions |
| 4 | +/-6 | 22 correct predictions |
| 5 | +/-3 | 35 correct predictions |

**Portfolio Scale** (structural progression):

| Year | Active Project Slots | Greenlight Slots per Quarter | Annual Budget |
|---|---|---|---|
| 1 (2003) | 3 | 1-2 | $80M |
| 2 (2004) | 4 | 2 | $90M |
| 3 (2005) | 4 | 2 | $100M |
| 4 (2006) | 5 | 2-3 | $120M |
| 5 (2007) | 5 | 2-3 | $140M |
| 6 (2008) | 5 | 2-3 | $120M (recession) |
| 7 (2009) | 6 | 3 | $135M |
| 8 (2010) | 6 | 3 | $155M |
| 9 (2011) | 6 | 3 | $175M |
| 10 (2012) | 6 | 3 | $190M |

**Studio Pool Growth**: The pitch pool expands as the campaign progresses and as the player's reputation grows.

| Year Range | Studios in Pitch Pool | New Studio Unlocks |
|---|---|---|
| Years 1-2 | 6-8 | Starting roster based on publisher reputation |
| Years 3-4 | 8-10 | 2 new studios attracted by portfolio performance |
| Years 5-6 | 10-12 | 2 new studios + potential return of departed studios (if industry reputation high) |
| Years 7-8 | 12-14 | Final wave of studios, including "legendary" Auteur if Prestige > 70 |
| Years 9-10 | 12-15 | Roster stabilized. Studio acquisitions become possible (2012 consolidation event). |

### Difficulty Curve

The difficulty curve is **stepped with escalating plateaus**:

```
Difficulty
    ^
    |                                              ___----  Year 9-10
    |                                         ___--        (Legacy Pressure)
    |                                    ___--
    |                               ___--                   Year 7-8
    |                          ___--                        (Specialization
    |                     ___--                              Demand)
    |               __---
    |          __---                                        Year 4-6
    |     __---                                            (Era Disruptions
    |  ---                                                  + Board Tightening)
    | -
    |--                                                    Year 1-3
    |                                                      (Learning / Grace)
    +---------------------------------------------------> Time
```

**Years 1-3 (Learning Phase)**: Board targets are lenient. Budget is sufficient. The studio pool is manageable. The player has room to experiment, make mistakes, and learn the systems. Era events are moderate (online gaming emergence, handheld market). The opening scenario (inherited cancellation) teaches the core decision loop.

**Years 4-6 (Disruption Phase)**: The Wii launches. Next-gen costs spike. The board tightens targets. The player must now actively strategize rather than react. This is where the five-minute loop gains complexity -- active projects are more numerous, interventions matter more, and the Era Engine starts punishing players who haven't adapted. Difficulty step: +40% over Years 1-3.

**Years 7-8 (Specialization Phase)**: The board demands that at least one Portfolio axis exceed 70. The player must commit to an identity. Digital distribution and DLC create new revenue possibilities but also new management complexity. Studio relationships that were neglected are now visibly hurting the pitch pool. Difficulty step: +30% over Years 4-6.

**Years 9-10 (Legacy Phase)**: All systems running at full complexity. The board demands clear results. The player's decade of decisions has created a publisher with a specific shape -- and the final years test whether that shape is sustainable. The final earnings call is the culmination. Difficulty step: +20% over Years 7-8.

### Pacing: New Mechanics Introduction Schedule

| Year | New Mechanic / System Introduced | Existing System Tested |
|---|---|---|
| 1 Q1 | Opening scenario (inherited cancellation). Greenlight Table tutorial. | -- |
| 1 Q2-Q4 | Intervention System introduced. First milestone review. | Greenlight Table |
| 2 | Portfolio Matrix becomes visible. Board targets introduced. Studio Trust becomes visible. | Greenlight + Interventions |
| 3 | Era Engine effects become significant (next-gen hardware). Counter-offers unlocked. | All Year 1-2 systems |
| 4 | Wii disruption. Casual market demand. Board axis specialization pressure begins. | Portfolio balancing under era pressure |
| 5 | Digital distribution slot. Budget game format available. | Adapting to new revenue models |
| 6 | DLC mandates available as intervention. Recession event. | Budget management under constraint |
| 7 | Live-service model option. Studio acquisition opportunities. | Long-term relationship payoffs (Trust compounds) |
| 8-10 | No new mechanics. Full mastery phase. All systems interact at maximum complexity. | Everything. This is the final exam. |

This pacing follows a principle I hold sacred: **introduce one system at a time, test it, then layer the next on top.** By Year 8, the player has internalized every system and the game stops teaching. The last three years are pure expression -- the player demonstrating mastery (or scrambling to survive) with a complete toolset.

---

## 6. Risk Assessment

### What Could Be Unfun

**Risk 1: Information Overload at the Greenlight Table**
The pitch dossier contains 10+ data fields. If the player feels like they're doing homework instead of making exciting bets, the core loop dies. **Mitigation**: Visual hierarchy. The Gut-Feel Indicator and Market Fit Score are the "headline" -- big, central, emotional. Budget and timeline are secondary. Risk factors are tertiary. The player can go deep if they want, but the surface read should be sufficient for a satisfying gut decision. Playtest focus: can a new player make a greenlight decision in under 30 seconds and feel good about it?

**Risk 2: Intervention Decision Fatigue**
With 4-6 active projects each needing a milestone review per quarter, the management phase could feel repetitive -- especially in mid-to-late game when projects are numerous. **Mitigation**: Not every project has a meaningful decision point every quarter. Projects running smoothly present a "Status: On Track" summary with no action required. Only troubled projects or critical milestones (alpha, beta, gold candidate) demand full review. Target: 1-2 meaningful intervention decisions per quarter, with the rest being quick confirmations. Playtest focus: does the management phase feel like interesting decisions or busywork?

**Risk 3: Portfolio Matrix Feels Arbitrary**
If the player doesn't understand why their Prestige dropped or how Populism works, the board evaluation feels like being punished by an opaque system. **Mitigation**: Every portfolio axis change is attributed to a specific game: "Project Titan (Metacritic 91) pushed Prestige to 62." Running tooltips show projected axis impacts before greenlighting. The board's annual feedback explicitly cites which games helped and hurt each axis. Playtest focus: after an annual review, can the player articulate why each axis moved?

**Risk 4: Studio Relationships Feel Mechanical**
If studios are just number bundles, the emotional core collapses. The player must feel like they're dealing with people, not stat sheets. **Mitigation**: Studios communicate through in-game emails, pitch presentations, and narrative events with distinct voices. Morale and trust changes are always accompanied by a narrative beat (a quote, an email, a news article). The studio archetypes are expressed through behavior patterns that feel characterful, not formulaic. Playtest focus: does the player remember studio names? Do they feel anything when a studio relationship changes?

**Risk 5: The Era Engine Feels Like a History Lesson, Not a Game System**
If era events are just "here's what happened in 2006, deal with it," they feel like external interruptions rather than strategic opportunities. **Mitigation**: Era events are telegraphed 1-2 quarters in advance through "rumor" news items ("Sources say Nintendo is developing a motion-control console for 2006 launch"). This gives the player time to position. Events should feel like waves to surf, not walls to hit. Playtest focus: does the player feel clever when they anticipate an era shift, or blindsided when one hits?

**Risk 6: The Game Is Too Long**
A 10-year campaign at 40 quarters could run 15-18 hours. If the back half feels like repetition of the front half with bigger numbers, players will bounce in Year 6. **Mitigation**: The pacing schedule introduces new systems through Year 7, and Years 8-10 are the mastery phase where long-term investments pay off. If playtesting reveals mid-game sag, the campaign can be compressed to 8 years (2003-2010) without losing core events. Playtest focus: track session drop-off. If more than 30% of playtesters stop between Years 5-7, the mid-game needs restructuring.

**Risk 7: Counter-Offers Are a Dominant Strategy**
If counter-offering at reduced budget is always better than full greenlight (because you save money and the quality penalty is mild), the Greenlight Table loses its tension. **Mitigation**: Counter-offer acceptance is probabilistic, gated by Trust. Low-Trust studios reject counter-offers 50% of the time and lose -8 Trust on rejection. The quality penalty for budget cuts is significant: a 30% budget cut costs ~15 Quality_Seed points, which can drop a potential 82 Metacritic game to a 67. Counter-offers should feel like a gamble, not a default. Playtest focus: what percentage of decisions are counter-offers? If it exceeds 40%, the penalties need tuning upward.

**Risk 8: The Opening Scenario Is a False Promise**
The inherited cancellation is a powerful narrative hook. If the rest of the game doesn't deliver moments of equivalent weight, the opening oversells the experience. **Mitigation**: Scripted narrative beats at Years 3, 5, 7, and 10 provide comparable dramatic moments (a studio's magnum opus shipping, a beloved studio closing, the final legacy montage). Between these, the emergent drama of the systems must carry the weight. The opening teaches the player what kind of decisions this game contains. The systems then generate more of them. Playtest focus: do playtesters cite specific emergent moments as memorable, or only the scripted ones?

### Playtest Priorities (Ordered)

1. **Greenlight Table feel-test (Week 1)**: Paper prototype. 6 pitch cards, 3 studio profiles, 2 greenlight slots. Does the decision feel weighty? Does the player agonize? Can they decide in 30 seconds? If this doesn't work, nothing else matters. This is the core loop. If the 30-second experience isn't compelling, no amount of content saves it.

2. **Intervention impact test (Week 2-3)**: Digital prototype. 3 active projects across 4 quarters. Does the player understand the tradeoffs? Do interventions feel meaningful or arbitrary? Does restraint feel like a valid strategy or a cop-out? Track: how often players intervene vs. hold, and whether their choices correlate with improved outcomes.

3. **Portfolio Matrix comprehension test (Week 3-4)**: Does the player understand why their axes moved? Can they articulate a strategy for the next year? If 3 out of 5 playtesters can't explain the Prestige/Profit/Populism system after 3 in-game years, the feedback loop is broken. Iterate on information display.

4. **Studio Relationship engagement test (Week 4-6)**: Full campaign slice (Years 1-4). Does the player form opinions about studios? Do they have a "favorite" studio? Do they feel betrayed by a cancellation or proud of a studio's success? Emotional engagement is the metric. If players talk about studios by name in post-session debrief, this system is working.

5. **Era Engine strategy test (Week 6-8)**: Full campaign (Years 1-10). Do experienced playtesters anticipate era shifts and position accordingly? Do they feel clever when a bet pays off? Does the Era Engine create distinct playthroughs (a player who bets on casual vs. one who bets on prestige)? Track strategic variance across 5+ full playthroughs.

6. **Full campaign pacing test (Week 8-12)**: Track session length, drop-off points, and player sentiment across the full 10-year campaign. Identify the "boredom trough" (every game has one) and restructure pacing around it. Target: 80% of committed playtesters finish the campaign within 3 sessions.

---

## 7. Economy Framework

### Budget Economy (Annual)

| Cost Category | Range | Notes |
|---|---|---|
| AAA Console Game (full budget) | $25M-$50M | Scales with era. 2003 AAA: ~$25M. 2012 AAA: ~$50M. |
| Mid-tier Console Game | $12M-$25M | The "safe bet" range. Reliable studio territory. |
| Indie / Digital-Only Game | $2M-$8M | Available after Year 5 (digital distribution era event). |
| Handheld Game | $5M-$15M | Available after Year 2 (DS/PSP launch). |
| Casual / Motion-Control Game | $8M-$18M | Available after Year 4 (Wii launch). |
| Marketing (per title) | $5M-$20M | Scales with projected revenue. Increases Populism. |
| Intervention Costs | $2M-$15M each | See intervention table. |

### Revenue Model (Per Shipped Title)

```
Base_Revenue = Units_Sold * Price_Per_Unit
Units_Sold   = (Quality_Score * 8000) + (Marketing_Spend * 3000) + (Franchise_Bonus * 50000) + (Era_Demand_Match * 40000)
Price_Per_Unit = $50 (console, 2003-2007) / $60 (console, 2008+) / $30 (handheld) / $15 (digital)

Net_Revenue = Base_Revenue - Development_Budget - Marketing_Spend
ROI = Net_Revenue / (Development_Budget + Marketing_Spend) * 100

DLC_Revenue (if mandated) = Base_Revenue * 0.15 to 0.30 (based on genre fit and audience size)
```

**Example Calculations**:

*Reliable Studio's Action Sequel, 2006, Xbox 360:*
- Quality_Score: 78 (Metacritic)
- Marketing: $12M
- Franchise_Bonus: Yes (2nd installment)
- Era_Demand: Moderate (action games stable)
- Units_Sold: (78 * 8000) + (12M/1M * 3000) + 50000 + 40000 = 624,000 + 36,000 + 50,000 + 40,000 = 750,000
- Revenue: 750,000 * $50 = $37.5M
- Budget: $22M + $12M marketing = $34M
- Net: $3.5M profit. ROI: 10.3%. Safe, thin margin.

*Auteur Studio's Original IP, 2007, PS3/360:*
- Quality_Score: 92
- Marketing: $15M
- Franchise_Bonus: No
- Era_Demand: High (prestige game era)
- Units_Sold: (92 * 8000) + (15M/1M * 3000) + 0 + (40000 * 1.5) = 736,000 + 45,000 + 0 + 60,000 = 841,000
- Revenue: 841,000 * $50 = $42.05M
- Budget: $35M + $15M = $50M
- Net: -$7.95M loss. But Prestige: +22. Trust: +15. This is an investment, not a revenue play.

*Volatile Studio's Casual Game, 2006, Wii:*
- Quality_Score: 74
- Marketing: $8M
- Franchise_Bonus: No
- Era_Demand: Very High (Wii casual explosion)
- Units_Sold: (74 * 8000) + (8M/1M * 3000) + 0 + (40000 * 2.5) = 592,000 + 24,000 + 0 + 100,000 = 716,000
- Revenue: 716,000 * $50 = $35.8M
- Budget: $12M + $8M = $20M
- Net: $15.8M profit. ROI: 79%. Populism: +14. The Wii bet pays.

### Board Satisfaction Formula

```
Board_Satisfaction = (Prestige_vs_Target * 0.30) + (Profit_vs_Target * 0.45) + (Populism_vs_Target * 0.25)

Where:
  Axis_vs_Target = min(Axis_Value / Target_Value, 1.5) * 100
  (Capped at 150% to prevent over-indexing on one axis)

Board_Satisfaction Thresholds:
  > 110: "Exceptional." Budget increase +10% next year. Bonus greenlight slot.
  90-110: "Satisfactory." No changes.
  70-89: "Concerning." Warning issued. One axis target increased by +5 next year.
  50-69: "Unacceptable." Yellow card. Budget cut 10%. Active slot removed.
  < 50: "Termination review." Two consecutive sub-50 scores = fired (game over with epilogue).
```

---

## 8. Failure State and Endgame

### Failure: Termination

The player is fired if Board_Satisfaction falls below 50 for two consecutive years. This triggers a **Termination Epilogue**: a montage showing what happened to each studio and each in-progress game after the player left. Games are finished by a successor (at reduced quality), cancelled, or acquired by rival publishers. Studios react based on Trust: high-Trust studios express regret; low-Trust studios celebrate. The player sees the ghost of the publisher they could have built.

### Success: Legacy Evaluation

Completing Year 10 triggers the **Legacy Montage**: a timeline of every game greenlit, every studio relationship, every intervention, and every era event, scored against three metrics:

| Legacy Category | Calculation | Rating |
|---|---|---|
| **Patron Score** | Average Trust across all studios + number of "magnum opus" games shipped | S/A/B/C/D |
| **Mogul Score** | Cumulative net revenue + final Profit axis value | S/A/B/C/D |
| **Architect Score** | Final Portfolio Matrix balance (how close to equal all three axes) + number of new IPs that scored above 80 | S/A/B/C/D |

The combined Legacy Grade (S through D) determines the closing narration: from "You built something that mattered" (S-rank) to "The board was right to worry" (D-rank). But the real endgame is the specific, personal montage -- the studio names, the game titles, the decisions -- that no two players share.

---

## 9. Open Questions for Playtesting

These are the questions I cannot answer from the design chair. They require players, prototypes, and observation.

1. **Is the Greenlight Table exciting or exhausting?** The core loop must feel like "opening a mystery box," not "reviewing homework." If playtesters spend more than 45 seconds per pitch on average, the information density is too high. If they spend less than 10 seconds, the decision doesn't feel meaningful. The sweet spot is 20-30 seconds of engaged evaluation followed by a committed stamp.

2. **Does restraint feel like a power move or a non-choice?** The Intervention System's restraint option is critical for Auteur relationships. But "do nothing" must feel like a *decision*, not the absence of one. Does the player feel tension when choosing to hold? Or do they feel like they're skipping a turn?

3. **How quickly do players internalize the Portfolio Matrix?** If the Prestige/Profit/Populism triangle isn't intuitive by Year 2, the board evaluations will feel like arbitrary punishment. Test: can a playtester predict how a specific greenlight decision will move the triangle before seeing the result?

4. **Do studio archetypes feel like characters or categories?** The Auteur must feel different from the Volatile in a way that goes beyond stat blocks. If playtesters refer to studios by archetype ("my Auteur studio") instead of by name ("Vanguard Interactive"), the personality system needs more expression.

5. **Is the Era Engine predictable enough to strategize around but surprising enough to create tension?** Too predictable: the game becomes a solved optimization puzzle. Too random: the player feels helpless. The 1-2 quarter rumor system is the proposed solution, but it needs calibration. Can playtesters correctly anticipate 60-70% of era shifts while being genuinely surprised by 30-40%?

6. **What's the right number of interventions per quarter?** Currently designed for 1-2 meaningful intervention decisions per quarter (with the rest being quick "on track" confirmations). If this number is too low, management feels shallow. If too high, it creates decision fatigue that overshadows the Greenlight Table. Track player engagement and frustration per management phase.

7. **Does the game need a mid-campaign dramatic event?** The opening (inherited cancellation) and the ending (legacy montage) are designed. But is there a natural mid-game inflection point, or does the campaign sag around Years 5-6? If playtesting reveals a pacing trough, a scripted mid-game event (e.g., a studio mutiny, a hostile takeover attempt, a once-in-a-generation pitch) could provide a structural spine.

8. **Is the counter-offer system balanced?** Counter-offers must be risky enough that full greenlights remain the default choice for most players. If counter-offers become a dominant strategy, the economic penalties need tuning. Track: what percentage of greenlight decisions are counter-offers, and do counter-offers produce worse outcomes on average?

9. **How does the game feel on a second playthrough?** The authored pitch backbone (50+ unique concepts) ensures variety, but the Era Engine follows the same historical progression. Does the player feel like they're exploring a new strategic space, or replaying a known script? If the latter, the random event pool and studio personality variance need more weight.

10. **Is getting fired satisfying?** The Termination Epilogue should feel like a meaningful ending, not a game-over screen. Playtesters who get fired should want to start over with new knowledge, not quit in frustration. The epilogue must show them *what they could have done differently* through the lens of what happened after they left.

---

## Appendix: Design Principles

These are the rules I'm holding myself to. When we're deep in production and someone asks "should we add this feature?" -- check it against these.

1. **The stamp is the game.** Every system exists to make the greenlight decision more interesting. If a feature doesn't change how the player evaluates a pitch, it doesn't belong in v1.

2. **Incomplete information is sacred.** The player must never have perfect data. The Gut-Feel Indicator, the Hidden Quality Seed, the probabilistic counter-offer acceptance -- these are not bugs in the design. They are the design. Certainty kills tension. Tension is the game.

3. **Studios are people, not content.** If we ever catch ourselves treating studios as interchangeable generators of "game objects," we've lost the emotional core. Every studio must have a name, a voice, a history, and a future that the player cares about. If they don't care, we've failed.

4. **The Era Engine is a strategic system, not a history textbook.** We're not simulating 2003-2012 for nostalgia. We're using historical forces as systemic modifiers that create strategic variety. The Wii launch isn't "remember the Wii?" -- it's a demand shock that reshapes viable strategies. If a player with zero game industry knowledge can engage with the Era Engine purely as a strategy system, we've succeeded.

5. **Restraint is a verb.** Choosing not to act must always feel like a deliberate choice with consequences. "Do nothing" is never the default. It's a bet on autonomy.

6. **The long game is the real game.** Hour 1 is the tutorial. Hour 10 is the real start. Hour 20 is where mastery emerges. We are building for the player who wants to see their decisions compound across a decade. If that player bounces at hour 8, we've paced it wrong.

7. **Show the humans behind the numbers.** Every stat change, every portfolio shift, every board evaluation should be accompanied by a human moment -- a quote, an email, a facial expression. The spreadsheet tells the player what happened. The human moment tells them why it matters.

---

*This document is a living design. It will change. The numbers will be rebalanced. The systems will be retuned. But the core question will not change: does the greenlight stamp carry weight? If the player hesitates every single time their hand hovers over that button, we've found the fun. Everything else is iteration.*

*-- REED*
