# GREENLIGHT — Release Plan

**Release Manager**: SHIP
**Version**: 1.0
**Target Platforms**: PC (Steam primary), Nintendo Switch (secondary)
**Build Type**: Premium single-player ($19.99 MSRP)
**Campaign Length**: 12-18 hours (10-year simulation)
**Status**: Pre-Production → Gold Master Planning

---

> "First impressions are permanent. Launch day defines a game's trajectory. There are no second chances for a botched release."
> — SHIP

---

## Document Purpose

This is the master release plan for GREENLIGHT. It covers every step from code complete to live on store. Every step. If it's not on this checklist, it doesn't happen. This document exists because launch days go wrong in predictable ways, and we've seen every one of them.

What's the rollback plan? We'll get to that. First, the checklist.

---

## 1. Master Launch Checklist

### 1.1 PRE-PRODUCTION PHASE (Weeks 1-20)

**Build Pipeline Setup**
- [ ] Unity 2022 LTS installed and configured
- [ ] Git repository with LFS configured for binary assets
- [ ] .gitignore excludes Library/, Temp/, Builds/, *.csproj, *.sln
- [ ] Branch strategy established (main/develop/feature/*)
- [ ] Unity Cloud Build or local build scripts configured
- [ ] Build versioning scheme defined (semantic versioning: MAJOR.MINOR.PATCH)
- [ ] Automated unit test pipeline running on commit
- [ ] Designer tools (Pitch Simulator, Campaign Visualizer) implemented

**Platform SDK Setup**
- [ ] Steam SDK integrated (Steamworks.NET or Facepunch.Steamworks)
- [ ] Steam AppID reserved (contact Valve early)
- [ ] Steam depot structure configured (base game, platform-specific builds)
- [ ] Nintendo Switch SDK obtained (requires dev approval)
- [ ] Switch devkit acquired and configured
- [ ] Platform-specific build targets tested (Windows x64, Switch ARM64)

**Save System Validation**
- [ ] JSON serialization tested with full campaign state
- [ ] Save data validation function implemented and tested
- [ ] Atomic file write tested (simulate power loss during save)
- [ ] Auto-save rotation implemented (keep 5 most recent)
- [ ] Manual save slots implemented (minimum 3, target 10)
- [ ] Save file size tested at Year 10 (worst-case: ~2MB, acceptable)
- [ ] Save corruption detection tested with deliberately malformed files
- [ ] Save migration system implemented for future patches

**Core Systems Validation** (Phase 1-6, Weeks 1-20)
- [ ] Greenlight Table: 30-second decision loop feels weighty (playtest validated)
- [ ] Intervention System: 15 interventions implemented, archetype-specific penalties working
- [ ] Portfolio Matrix: Prestige/Profit/Populism calculations match design doc
- [ ] Studio AI: 12-15 studios with distinct archetype behaviors
- [ ] Era Engine: 20+ core events (2003-2012), rumor system working
- [ ] Revenue calculations: Metacritic resolution, unit sales, ROI formulas validated
- [ ] Board evaluator: Annual reviews, target escalation, satisfaction scoring
- [ ] Trust/morale systems: Accumulation, decay, inter-studio reputation cascades

**Content Authoring Complete**
- [ ] 50+ pitch concepts authored as ScriptableObjects
- [ ] 12-15 studio definitions authored with archetype assignments
- [ ] 15+ intervention definitions with archetype-specific modifiers
- [ ] 20+ era events with genre/platform demand modifiers
- [ ] Opening scenario (inherited cancellation) scripted and tested
- [ ] Legacy montage system implemented (endgame visualization)
- [ ] Termination epilogue implemented (fired ending)

---

### 1.2 ALPHA PHASE (Week 21)

**Feature Complete Milestone**
- [ ] All core systems implemented and integrated
- [ ] Full 10-year campaign playable start to finish
- [ ] All UI screens implemented (may be placeholder art)
- [ ] Save/load working across full campaign
- [ ] No placeholder text in player-facing strings

**Alpha Build Requirements**
- [ ] PC build compiles without errors or warnings
- [ ] Switch build compiles (if dev approval obtained)
- [ ] Unit test suite passing (target: 80% coverage on simulation layer)
- [ ] Integration tests passing (full quarter simulation, multi-year campaign)
- [ ] No critical bugs (crashes, save corruption, soft-locks)

**Alpha Playtest**
- [ ] Internal playtest: 3-5 testers complete full campaign
- [ ] Playtest survey covers: pacing, decision clarity, studio personality engagement
- [ ] Track drop-off points (if >30% quit between Years 5-7, mid-game needs restructuring)
- [ ] Collect balance feedback (board targets too harsh/lenient? intervention costs fair?)
- [ ] Identify "boredom trough" (every game has one)

**Alpha Exit Criteria**
- [ ] Full campaign completable without crashes
- [ ] Save files stable across multiple sessions
- [ ] Core loop (greenlight decision) validated as fun by 80% of playtesters
- [ ] No game-breaking bugs in bug database

---

### 1.3 BETA PHASE (Weeks 22-23)

**Content Lock**
- [ ] All pitch concepts final (no new concepts added post-beta)
- [ ] All studio definitions final
- [ ] All era events final
- [ ] All intervention definitions final
- [ ] String freeze for localization (if applicable)

**Polish Pass**
- [ ] UI animations implemented (stamp animation, portfolio chart transitions)
- [ ] Audio implementation: SFX for all player actions (stamp, interventions, phase transitions)
- [ ] Audio implementation: Ambient music (corporate office hum, boardroom tension)
- [ ] Audio implementation: Studio-specific pitch music signatures
- [ ] Visual polish: Final UI art pass, color palette consistency
- [ ] Accessibility: Text scaling options, colorblind-friendly palette
- [ ] Accessibility: Controller support fully implemented and tested (Switch readiness)

**Performance Validation**
- [ ] PC performance: 60 FPS UI, <100ms simulation per quarter
- [ ] Switch performance: 30 FPS UI (handheld mode worst-case), <200ms simulation
- [ ] Memory profiling: Confirm <90MB total footprint
- [ ] GC allocation audit: No per-frame allocations in UI code
- [ ] Load time testing: Boot to main menu <5 seconds, save load <2 seconds

**Beta Build Distribution**
- [ ] Steam Beta branch configured (password-protected)
- [ ] Beta keys generated (100+ for external testers)
- [ ] Beta tester recruitment: Target 50-100 external testers
- [ ] Beta feedback form: Google Forms or Typeform survey
- [ ] Beta NDA if applicable (or public beta for marketing purposes)

**Beta Playtest Focus**
- [ ] Balance tuning: Board satisfaction thresholds, intervention costs, quality seed formulas
- [ ] Edge case hunting: Studio AI bugs, era event conflicts, save corruption scenarios
- [ ] Platform testing: Switch controller input, PC keyboard shortcuts
- [ ] Completion rate: Track percentage of beta testers who finish Year 10
- [ ] Bug triage: P0 (crash/corruption) → P1 (gameplay-breaking) → P2 (polish) → P3 (nice-to-have)

**Beta Exit Criteria**
- [ ] Zero P0 bugs
- [ ] Zero P1 bugs
- [ ] P2 bugs triaged (fix vs. defer to patch 1.1)
- [ ] 60%+ beta completion rate (if lower, revisit pacing)
- [ ] Balance validated by external playtesters (no death spiral reports, board feels fair)

---

### 1.4 RELEASE CANDIDATE PHASE (Week 24)

**Code Freeze**
- [ ] Last commit before gold tagged in Git (e.g., `v1.0.0-rc1`)
- [ ] No new features post-RC
- [ ] Only critical bug fixes allowed (must be approved by SHIP + BYTE)

**Build Pipeline Finalization**
- [ ] PC build: Windows x64, standalone executable
- [ ] PC build: Steam depot upload tested (use beta branch for dry run)
- [ ] Switch build: ARM64, retail-ready build (if approved)
- [ ] Build versioning embedded in executable (visible in main menu, logs)
- [ ] Build signing configured (if applicable for platform)

**Final QA Pass**
- [ ] Clean machine testing: Install on fresh Windows 10/11 machine with no dev tools
- [ ] Clean machine testing: Verify all dependencies bundled (no missing DLLs)
- [ ] Clean machine testing: Test save/load on clean machine (no pre-existing saves)
- [ ] Platform-specific QA: Switch devkit testing (performance, save integrity, controller edge cases)
- [ ] Regression testing: Re-run all critical path tests (greenlight, intervention, shipping games, annual reviews)
- [ ] Soak testing: Leave game running for 4+ hours, verify no memory leaks or performance degradation

**Age Rating Submission** (See Section 6)
- [ ] ESRB rating application submitted (PC/Switch, expect E or E10+)
- [ ] IARC questionnaire completed (digital storefronts)
- [ ] Age rating assets prepared (rating icons for store pages, packaging)

**RC Exit Criteria**
- [ ] Zero P0 or P1 bugs
- [ ] Clean machine install successful
- [ ] Performance targets met on all platforms
- [ ] Age ratings obtained or in progress
- [ ] Build approved by SHIP, BYTE, CLOCK

---

### 1.5 GOLD MASTER (Week 25)

**Gold Master Declaration**
- [ ] Final build tagged in Git (e.g., `v1.0.0-gold`)
- [ ] Build hash recorded in release notes
- [ ] Gold master build archived in secure location (3 copies: local, cloud, external drive)
- [ ] Gold master build uploaded to Steam (set to "Coming Soon", not live)
- [ ] Gold master build submitted to Nintendo (if applicable, 4-6 week approval process)

**Store Page Preparation** (See Section 3)
- [ ] Steam store page configured (see checklist in Section 3.1)
- [ ] Nintendo eShop page configured (if applicable, see checklist in Section 3.2)
- [ ] Store page assets uploaded and reviewed
- [ ] Store page SEO optimized (tags, keywords, description)

**Marketing Assets Finalized** (Coordinate with HYPE)
- [ ] Launch trailer approved and uploaded
- [ ] Capsule art (Steam header, library tiles) finalized
- [ ] Screenshots (10+, showcasing greenlight table, milestone reviews, portfolio matrix)
- [ ] Key art (1920x1080, vertical and horizontal variants)
- [ ] Presskit created (EPK with fact sheet, screenshots, trailer, contact info)

**Press & Influencer Outreach**
- [ ] Press list compiled (PC Gamer, RPS, Polygon, Kotaku, IGN, GameSpot, Eurogamer)
- [ ] Influencer list compiled (YouTubers/streamers focused on management sims, strategy games)
- [ ] Review keys generated (100+ Steam keys)
- [ ] Review embargo set (lift date = launch day or 1 day prior)
- [ ] Press emails sent 2 weeks before launch
- [ ] Follow-up emails sent 1 week before launch

**Community Preparation**
- [ ] Discord server launched (or subreddit created)
- [ ] Social media accounts active (Twitter/X, Reddit)
- [ ] Devlog series published ("Greenlight Diaries" — fictional publisher memos)
- [ ] Steam community hub moderated

---

### 1.6 LAUNCH DAY (Week 26)

**T-Minus 72 Hours**
- [ ] Final smoke test on live Steam beta branch
- [ ] Confirm all store pages live (Steam, Nintendo if applicable)
- [ ] Confirm payment processing configured (Steam handles this, but verify)
- [ ] Confirm regional pricing configured (Steam: USD base, auto-convert with manual review)
- [ ] Backup plan confirmed: Who owns launch day deployment? (Primary: SHIP, Backup: BYTE)

**T-Minus 24 Hours**
- [ ] Launch day timeline distributed to team (see Section 5)
- [ ] Emergency contact sheet distributed (Steam support, platform holders, PR contacts)
- [ ] Day-one patch ready (if needed, see Section 4.3)
- [ ] Monitoring dashboard configured (Steam stats, social media mentions, bug reports)

**Launch Day Hour-by-Hour** (See Section 5 for detailed timeline)
- [ ] 00:00 UTC: Game goes live on Steam (auto-release or manual flip)
- [ ] 00:15 UTC: Smoke test live build (purchase with test account, verify download/install/launch)
- [ ] 01:00 UTC: Monitor initial player reports (Steam forums, Discord, social media)
- [ ] 06:00 UTC: First metrics check (units sold, concurrent players, review scores)
- [ ] 12:00 UTC: Mid-day check-in (any critical bugs reported? hotfix needed?)
- [ ] 18:00 UTC: Evening metrics (peak concurrent players, sentiment analysis)
- [ ] 23:59 UTC: End-of-day report (sales, reviews, bug triage)

**Launch Day Responsibilities**
- [ ] SHIP: On-call for critical deployment issues, rollback decisions
- [ ] BYTE: On-call for critical bug fixes, hotfix deployment
- [ ] CRASH: Monitoring bug reports, triaging by priority
- [ ] HYPE: Monitoring social media, responding to community questions
- [ ] CLOCK: Tracking metrics, coordinating team communication

---

### 1.7 POST-LAUNCH (Weeks 27-30)

**Week 1 Post-Launch**
- [ ] Daily bug triage meetings
- [ ] Hotfix deployed if P0/P1 bugs discovered (see Section 4.3)
- [ ] Community manager responding to player feedback (Steam forums, Discord, Reddit)
- [ ] Metrics analysis: Sales velocity, review score trends, refund rates
- [ ] Player behavior analytics: Drop-off points, average session length, campaign completion rates

**Week 2 Post-Launch**
- [ ] Patch 1.1 planning based on feedback (balance tuning, QoL improvements)
- [ ] Review aggregation: Metacritic tracking (this game IS about Metacritic, ironically)
- [ ] Influencer coverage tracking (YouTube views, Twitch streams)

**Month 2: Patch 1.1** (See Section 7)
- [ ] Bug fixes from launch feedback
- [ ] Balance tuning (board targets, intervention costs based on player data)
- [ ] QoL features: Fast-forward quarter button, undo last decision

**Month 4: Patch 1.2**
- [ ] New studio archetypes (2-3 additional)
- [ ] New interventions (5 additional)
- [ ] Expanded random event pool

**Month 8: Expansion Planning**
- [ ] "The Indie Boom" expansion scoped (extend campaign to 2015, add crowdfunding mechanics)

---

## 2. Build Pipeline Documentation

### 2.1 PC (Steam) Build Process

**Prerequisites**
- Unity 2022 LTS installed
- Steamworks SDK integrated
- Steam AppID configured in Unity project settings

**Build Steps**
1. Open Unity project
2. Set build target: File → Build Settings → PC, Mac, Linux Standalone
3. Architecture: x86_64 (64-bit only)
4. Scripting backend: Mono (faster iteration) or IL2CPP (smaller binary)
5. Code optimization: Master (release) or Debug (for verbose logs)
6. Build location: `/Builds/PC/GREENLIGHT_v{VERSION}/`
7. Build name: `GREENLIGHT.exe`

**Post-Build Validation**
1. Run `GREENLIGHT.exe` on clean Windows machine (no Unity installed)
2. Verify main menu loads without errors
3. Start new campaign, play through Quarter 1
4. Save game, exit, relaunch, load save
5. Check logs for errors: `%APPDATA%/../LocalLow/{CompanyName}/GREENLIGHT/Player.log`

**Steam Depot Upload**
1. Use SteamCMD or Steamworks GUI
2. Upload to depot ID (assigned by Valve)
3. Set build live on beta branch first (test before public release)
4. Command example:
   ```
   steamcmd +login {username} +app_build {appid} {vdf_file} +quit
   ```

**Automation** (Optional)
- Unity Cloud Build: Configure build trigger on Git tag (e.g., `v1.0.0` → auto-build)
- Jenkins/GitHub Actions: CI/CD pipeline for automated testing + build

---

### 2.2 Nintendo Switch Build Process

**Prerequisites**
- Nintendo Switch SDK installed (requires dev approval)
- Switch devkit hardware
- Unity Switch build support module installed
- Nintendo Developer Portal account

**Build Steps**
1. Open Unity project
2. Set build target: File → Build Settings → Nintendo Switch
3. Architecture: ARM64
4. Scripting backend: IL2CPP (required by platform)
5. Code stripping: Medium (balance size vs. functionality)
6. Build location: `/Builds/Switch/GREENLIGHT_v{VERSION}/`
7. Build name: `GREENLIGHT.nsp` (Nintendo Submission Package)

**Switch-Specific Settings**
- Application ID: Assigned by Nintendo
- Title ID: Assigned by Nintendo
- Icon assets: 256x256 PNG (required by Nintendo)
- Save data size: 2MB allocation (campaign saves)
- Supported languages: English (launch), expand post-launch
- Age rating: IARC (handled via Nintendo Developer Portal)

**Devkit Testing**
1. Install `.nsp` on Switch devkit via USB or network
2. Test in handheld mode (worst-case performance: 720p)
3. Test in docked mode (1080p)
4. Verify controller input (Joy-Con, Pro Controller)
5. Test suspend/resume (Switch home button behavior)
6. Test save integrity across suspend/resume cycles

**Submission to Nintendo**
- Use Nintendo Developer Portal
- Upload `.nsp` build
- Submit age rating questionnaire (IARC)
- Submit store page assets (screenshots, trailer, description)
- Approval timeline: 4-6 weeks (plan accordingly)

---

### 2.3 Build Versioning Scheme

**Format**: `MAJOR.MINOR.PATCH` (Semantic Versioning)

**Examples**:
- `1.0.0`: Gold master launch build
- `1.0.1`: Hotfix (critical bug fix, no new features)
- `1.1.0`: Patch 1.1 (balance tuning, QoL features)
- `1.2.0`: Patch 1.2 (new studios, interventions)
- `2.0.0`: Major expansion ("The Indie Boom")

**Version Embedding**
- Version string embedded in main menu UI
- Version string in Player.log on game launch
- Version number in save file header (for migration)

**Git Tagging**
- Tag format: `v1.0.0`, `v1.0.1`, etc.
- Annotated tags: `git tag -a v1.0.0 -m "Gold master release"`
- Push tags: `git push origin v1.0.0`

---

## 3. Platform Configuration Checklists

### 3.1 Steamworks Configuration

**App Setup**
- [ ] Steam AppID reserved (contact Valve, 1-2 week turnaround)
- [ ] App admin access configured (assign team members)
- [ ] App type: Game (not DLC, not demo)
- [ ] Release state: Coming Soon → Released (on launch day)

**Store Page**
- [ ] Game name: "GREENLIGHT"
- [ ] Tagline: "You don't make games — you decide which games get made."
- [ ] Short description (300 chars): "You're a game publisher executive in the mid-2000s. AI studios pitch you games. You decide who gets funded, who gets cancelled, and who gets a sequel. Every greenlight is a bet — on talent, on trends, on yourself."
- [ ] Long description (See Section 3.1.1)
- [ ] Developer: {Studio Name}
- [ ] Publisher: {Studio Name}
- [ ] Release date: {Launch Date}
- [ ] Platforms: Windows
- [ ] Languages: English (audio and interface)

**Store Assets**
- [ ] Capsule image (header): 460x215 PNG (greenlight stamp on pitch document)
- [ ] Library capsule: 600x900 PNG
- [ ] Library hero: 3840x1240 PNG
- [ ] Screenshots: Minimum 5, recommend 10 (1920x1080 PNG)
  - Greenlight Table with 3 pitches
  - Milestone review screen (studio morale/budget visible)
  - Portfolio matrix radar chart
  - Board meeting scene
  - Studio relationship panel
  - Era event notification (Wii launch example)
  - Legacy montage (endgame screen)
  - Opening scenario (inherited cancellation)
- [ ] Trailer: 60-90 seconds, MP4, 1080p (upload to Steam CDN)
- [ ] Background video (optional): 30-second loop for store page

**Pricing**
- [ ] Base price: $19.99 USD
- [ ] Regional pricing: Auto-convert with manual review for key markets (EUR, GBP, JPY)
- [ ] Launch discount: Consider 10% off for first week (coordinate with HYPE)

**Tags** (Choose 10-15, prioritize discoverability)
- [ ] Simulation
- [ ] Management
- [ ] Strategy
- [ ] Choices Matter
- [ ] Singleplayer
- [ ] Economy
- [ ] Decision-Making
- [ ] 2000s
- [ ] Indie
- [ ] Replay Value
- [ ] Story Rich
- [ ] Atmospheric
- [ ] Stylized
- [ ] Tutorial

**System Requirements**
- [ ] Minimum:
  - OS: Windows 10 (64-bit)
  - Processor: Intel Core i3 / AMD equivalent
  - Memory: 4 GB RAM
  - Graphics: Integrated graphics (Intel HD 4000 or better)
  - Storage: 500 MB available space
- [ ] Recommended:
  - OS: Windows 10/11 (64-bit)
  - Processor: Intel Core i5 / AMD equivalent
  - Memory: 8 GB RAM
  - Graphics: Dedicated GPU (any modern card)
  - Storage: 500 MB available space

**Steamworks Features**
- [ ] Steam Cloud: Enabled (save files auto-sync)
- [ ] Steam Cloud quota: 100 MB allocated (our saves are ~2MB max)
- [ ] Steam Achievements: 30-40 achievements defined (see Section 3.1.2)
- [ ] Steam Stats: Configured for tracking (games greenlit, studios championed, average Metacritic)
- [ ] Steam Trading Cards: Optional (6 cards + 3 badges, if approved by Valve)
- [ ] Steam Workshop: Not applicable (no user-generated content in v1)

**Community Hub**
- [ ] Forums enabled
- [ ] Discussions moderated (assign community manager)
- [ ] News hub active (post devlogs, patch notes)

#### 3.1.1 Steam Long Description Template

```
GREENLIGHT is a publisher management simulation set in the mid-2000s game industry. You don't make games — you decide which games get made.

THE GREENLIGHT TABLE
Every quarter, studios pitch you their games. Each pitch is a dossier: concept, budget, timeline, risk factors, and a gut-feel indicator that's never fully reliable. You can greenlight, pass, or counter-offer. You never have enough funding slots for every good pitch. Saying no to a great game is as defining as saying yes.

THE INTERVENTION SYSTEM
Once a game is in development, you receive milestone reviews. Do you force multiplayer to chase the Xbox Live market? Demand a movie tie-in for marketing synergy? Cut the open world to save budget? Every intervention has a visible morale cost and a hidden quality modifier you won't see until reviews hit.

THE STUDIO RELATIONSHIP WEB
Studios are not interchangeable. They have personalities, track records, and opinions of you. The auteur studio resents interference but will pitch you their magnum opus if you earn their trust. The sequel machine prints money but won't innovate unless pushed. The volatile indie needs mentorship or they'll implode. Studios talk to each other. Your reputation is currency.

THE ERA ENGINE
The game simulates 2003-2012 as a living ecosystem. The Xbox 360 launches. The Wii disrupts everything. DLC becomes viable. These aren't flavor text — they're systemic modifiers that change which pitches succeed and which strategies work. The player who greenlit a Wii-exclusive party game in 2006 looks like a genius. The one who bet on a PS3-exclusive open-world RPG is sweating through earnings calls.

THE PORTFOLIO MATRIX
Your publisher has a portfolio identity across three axes: Prestige (critical acclaim), Profit (revenue), and Populism (mainstream appeal). Every game you ship moves the needle on all three. A 94 Metacritic indie is Prestige gold but Profit poison. A movie tie-in that scores 62 and sells 5 million is the opposite. The board sets targets. You cannot maximize all three.

WHAT MASTERY LOOKS LIKE
Hour 1: You greenlight games that sound cool.
Hour 10: You read the Era Engine, maintain a balanced portfolio, and survive earnings calls.
Hour 20: You orchestrate an ecosystem. You cultivated the Auteur Collective for three years with hands-off publishing so they'd offer their magnum opus in 2007 — right when the next-gen install base hits critical mass. That's not luck. That's portfolio management.

FEATURES
- 10-year campaign (2003-2012) spanning 40 quarters
- 50+ unique game pitches from 12-15 persistent AI studios
- 15+ intervention actions with studio-archetype-specific consequences
- Historical industry events that reshape the market every year
- Three-axis portfolio management (Prestige, Profit, Populism)
- Multiple endings based on your publisher's legacy
- 12-18 hour playtime for full campaign
```

#### 3.1.2 Steam Achievements (Sample)

**Campaign Milestones**
- [ ] "First Greenlight" — Approve your first pitch
- [ ] "Survived Year 1" — Complete 2003 without getting fired
- [ ] "Golden Anniversary" — Celebrate your 50th greenlit game
- [ ] "Decade of Decisions" — Complete the full campaign (2003-2012)

**Portfolio Achievements**
- [ ] "Prestige Publisher" — Reach Prestige 75+
- [ ] "Profit Maximizer" — Reach Profit 75+
- [ ] "Popular Vote" — Reach Populism 75+
- [ ] "Triple Threat" — Have all three axes above 65 in the same year
- [ ] "Metacritic Darling" — Ship a game with 90+ Metacritic
- [ ] "Blockbuster" — Ship a game that sells 5 million+ units

**Studio Relationships**
- [ ] "Auteur's Trust" — Earn Trust 30+ with an Auteur studio
- [ ] "Magnum Opus" — Greenlight an Auteur's exclusive magnum opus pitch
- [ ] "Studio Savior" — Reverse a Fading studio's capability decay
- [ ] "Loyal Partnership" — Work with the same studio for 5+ consecutive years

**Decisions & Interventions**
- [ ] "Hands Off" — Complete a full year without any interventions
- [ ] "Micromanager" — Apply 20+ interventions in a single year
- [ ] "The Axe Falls" — Cancel a project mid-development
- [ ] "Against the Grain" — Greenlight a pitch with "Low" market fit that scores 85+

**Era Mastery**
- [ ] "Wii Wizard" — Greenlight a Wii-exclusive game in 2006 that scores 80+
- [ ] "Next-Gen Pioneer" — Greenlight an Xbox 360 launch title
- [ ] "Digital Visionary" — Greenlight a digital-only game in 2007
- [ ] "DLC Innovator" — Mandate DLC on a game that generates 30%+ post-launch revenue

**Special Endings**
- [ ] "The Patron" — Earn S-rank Patron Score in legacy evaluation
- [ ] "The Mogul" — Earn S-rank Mogul Score in legacy evaluation
- [ ] "The Architect" — Earn S-rank Architect Score in legacy evaluation
- [ ] "Terminated" — Get fired by the board

---

### 3.2 Nintendo eShop Configuration (If Applicable)

**Prerequisites**
- [ ] Nintendo Developer account approved
- [ ] Age rating obtained (IARC via Nintendo portal)
- [ ] Build approved by Nintendo (4-6 week timeline)

**Store Page**
- [ ] Game title: "GREENLIGHT"
- [ ] Subtitle: "Publisher Management Simulation"
- [ ] Short description (100 chars): "Decide which games get made. Manage studios, interventions, and a decade of industry upheaval."
- [ ] Long description (500 chars, adapted from Steam description)
- [ ] Developer: {Studio Name}
- [ ] Publisher: {Studio Name}
- [ ] Release date: {Launch Date}
- [ ] Supported play modes: TV mode, Tabletop mode, Handheld mode
- [ ] Number of players: 1
- [ ] Supported languages: English

**Store Assets**
- [ ] Icon: 256x256 PNG
- [ ] Screenshots: 8-10 images (1280x720 PNG, handheld resolution)
- [ ] Trailer: 60-90 seconds, MP4, 1080p (upload via Nintendo portal)

**Pricing**
- [ ] Base price: $19.99 USD (parity with Steam)
- [ ] Regional pricing: Nintendo handles conversion

**Age Rating**
- [ ] IARC questionnaire completed
- [ ] Expected rating: E (Everyone) or E10+
- [ ] Rating icons uploaded to store page

**Cloud Saves**
- [ ] Nintendo Switch Online cloud save support: Enabled
- [ ] Save data size: 2MB allocated

---

### 3.3 Regional Pricing Strategy

**Base Price**: $19.99 USD

**Steam Regional Pricing** (Auto-convert with manual review)
- EUR: €18.99
- GBP: £16.99
- JPY: ¥2,200
- AUD: $29.95
- CAD: $24.99
- BRL: R$ 65
- RUB: 899₽ (if still supported)

**Rationale**: Premium indie price point. Not budget ($9.99), not AAA ($39.99). Comparable to Game Dev Tycoon ($14.99) but with deeper systems justifying $5 premium.

**Launch Discount** (Optional)
- 10% off first week (coordinate with HYPE for marketing push)
- Reduces price to $17.99, lowers barrier for impulse purchases

---

## 4. Rollback & Hotfix Procedures

### 4.1 Rollback Plan

**Scenario**: Critical issue discovered post-launch (crash on startup, save corruption, game-breaking bug).

**Decision Criteria**: Rollback if issue affects >10% of players OR prevents game launch/save functionality.

**Rollback Authority**: SHIP (primary), BYTE (backup)

**Rollback Steps (Steam)**:
1. Identify last known good build (e.g., RC2 or Gold Master)
2. Set Steam depot to previous build version
3. Command: `steamcmd +set_live_build {previous_build_id} +quit`
4. Verify rollback: Download game with fresh Steam account, test functionality
5. Post announcement in Steam community hub: "We've temporarily rolled back to a previous build while we address [issue]. ETA for fix: [timeframe]."
6. Monitor player reports to confirm issue is resolved by rollback

**Rollback Steps (Nintendo Switch)**:
- Note: Nintendo does not support instant rollback. If critical issue found post-launch, must submit hotfix patch and wait for approval (2-4 weeks).
- Emergency mitigation: Post notice on eShop page warning players of known issue and ETA for fix.

**Communication Template (Steam Community Post)**:
```
URGENT UPDATE

We've identified a critical issue affecting [% of players / specific scenario].
To ensure everyone can play, we've temporarily rolled back to the previous stable build.

What this means:
- The game is fully playable
- [Specific issue] is resolved in this build
- We are working on a permanent fix

ETA for patched build: [Timeframe]

Thank you for your patience. Your saves are safe.

— The GREENLIGHT Team
```

---

### 4.2 Hotfix Development Process

**Hotfix Triggers**:
- P0 bug discovered (crash, save corruption, soft-lock)
- >5% of players reporting same issue within 24 hours
- Metacritic review mentions game-breaking bug

**Hotfix Workflow**:
1. **Triage** (SHIP + CRASH): Confirm bug reproducibility, assess severity
2. **Fix** (BYTE): Implement minimal fix (no new features, only bug resolution)
3. **Test** (CRASH): Reproduce original bug, verify fix, regression test critical paths
4. **Build**: Increment version (e.g., `1.0.0` → `1.0.1`)
5. **Deploy**: Upload to Steam, set live immediately (if critical) or schedule for off-peak hours
6. **Communicate**: Post patch notes in Steam community hub, Discord, social media

**Hotfix Testing Checklist** (Expedited QA)
- [ ] Original bug no longer reproducible
- [ ] Save/load functionality intact
- [ ] Core loop (greenlight decision) unaffected
- [ ] No new crashes introduced
- [ ] Clean machine install test (if fix affects dependencies)

**Hotfix Deployment Timeline**:
- Critical (P0): Fix within 24-48 hours, deploy immediately
- High (P1): Fix within 1 week, deploy during next maintenance window

---

### 4.3 Day-One Patch Strategy

**Philosophy**: Ship gold master stable. Day-one patch is for polish, not critical fixes.

**Acceptable Day-One Patch Content**:
- Balance tuning based on late beta feedback (board targets, intervention costs)
- Minor UI polish (tooltip clarity, text fixes)
- Performance optimizations discovered post-gold

**Unacceptable Day-One Patch Content**:
- Critical bug fixes (these should be in gold master)
- New features (scope creep, red flag for reviews)

**Day-One Patch Process**:
1. Finalize patch content 1 week before launch
2. Build patch as `1.0.1` (incremental over `1.0.0` gold)
3. Upload to Steam depot, set to auto-deploy on launch day
4. Patch size target: <50 MB (small download, minimal player friction)

**Patch Notes Template**:
```
GREENLIGHT — Patch 1.0.1 (Day-One Update)

BALANCE ADJUSTMENTS
- Board Prestige target reduced by 5 in Years 1-3 (grace period extended)
- "Force Multiplayer" intervention cost reduced from $8M to $6M
- Auteur studio morale penalty for interventions reduced from 2x to 1.5x

UI IMPROVEMENTS
- Tooltip clarity: Intervention morale impacts now show exact numbers
- Portfolio matrix: Axis labels now visible at all resolutions

PERFORMANCE
- Reduced UI rebuild frequency during milestone reviews (10-15% frame time improvement)

Thank you for playing GREENLIGHT. Your feedback shapes this publisher's legacy.
```

---

## 5. Launch Day Timeline (Hour-by-Hour)

**Date**: {Launch Day}
**Platform**: Steam (primary), Nintendo eShop (if approved)
**Launch Time**: 00:00 UTC (global simultaneous release)

### Pre-Launch (T-Minus 24 Hours)

**17:00 UTC (T-24h)**
- [ ] SHIP: Final smoke test on Steam beta branch
- [ ] BYTE: Confirm build version matches gold master (`v1.0.0`)
- [ ] HYPE: Schedule launch day social media posts
- [ ] CLOCK: Distribute launch day timeline to team

**20:00 UTC (T-4h)**
- [ ] SHIP: Verify Steam store page live and accessible (still "Coming Soon")
- [ ] BYTE: Monitor build upload status (all depots green)
- [ ] CRASH: Prepare bug triage spreadsheet
- [ ] HYPE: Pre-launch tweet/post scheduled ("4 hours until launch")

**23:00 UTC (T-1h)**
- [ ] SHIP: Final pre-flight check (store page, pricing, release state)
- [ ] All hands on deck: Team on standby for launch

---

### Launch Day (Hour-by-Hour)

**00:00 UTC — LAUNCH**
- [ ] SHIP: Flip Steam release state from "Coming Soon" to "Released"
- [ ] SHIP: Verify game appears in "New Releases" section
- [ ] HYPE: Post launch announcement (Twitter/X, Discord, Reddit, Steam)
- [ ] HYPE: Launch trailer live on YouTube

**00:15 UTC — Smoke Test**
- [ ] SHIP: Purchase game with test Steam account
- [ ] SHIP: Download and install (verify build version)
- [ ] SHIP: Launch game, start new campaign, play through Q1 2003
- [ ] SHIP: Save game, exit, relaunch, load save
- [ ] SHIP: Confirm no critical issues

**01:00 UTC — Initial Monitoring**
- [ ] CRASH: Monitor Steam forums for bug reports
- [ ] CRASH: Monitor Discord #bug-reports channel
- [ ] HYPE: Monitor social media mentions (@mentions, keywords: "GREENLIGHT bug", "GREENLIGHT crash")
- [ ] SHIP: Review initial player count (Steam stats)

**03:00 UTC — Night Shift Check-In**
- [ ] SHIP: Any P0 bugs reported? (If yes, activate hotfix procedure)
- [ ] CRASH: Triage reported bugs (P0/P1/P2/P3)
- [ ] HYPE: Respond to player questions on Steam/Discord

**06:00 UTC — Morning Metrics**
- [ ] CLOCK: Pull Steam stats (units sold, concurrent players, revenue)
- [ ] CLOCK: Check review scores (Steam reviews, early Metacritic if available)
- [ ] SHIP: Decision point: Any critical issues requiring rollback? (Hope: NO)

**09:00 UTC — Team Sync (Daily Standup)**
- [ ] All hands: 15-minute standup via Slack/Discord
- [ ] CRASH: Report bug triage status
- [ ] SHIP: Report deployment status
- [ ] HYPE: Report community sentiment
- [ ] CLOCK: Report metrics

**12:00 UTC — Mid-Day Check**
- [ ] CRASH: Re-triage bugs (any new P0/P1 reports?)
- [ ] SHIP: Monitor server load (if applicable, though this is single-player offline)
- [ ] HYPE: Community management (respond to threads, thank reviewers)

**18:00 UTC — Evening Peak**
- [ ] CLOCK: Pull peak concurrent player count (EU/US evening overlap)
- [ ] CRASH: Final bug triage for the day
- [ ] SHIP: Assess: Do we need a hotfix this week?

**23:59 UTC — End of Day Report**
- [ ] CLOCK: Compile launch day metrics (sales, CCU, reviews, refunds)
- [ ] SHIP: Write launch day post-mortem (what went right, what went wrong)
- [ ] Team: Celebrate (or commiserate, depending on metrics)

---

### Launch Day Responsibilities Matrix

| Time (UTC) | SHIP | BYTE | CRASH | HYPE | CLOCK |
|---|---|---|---|---|---|
| 00:00 | Deploy | Standby | Monitor | Announce | Track |
| 00:15 | Smoke test | Standby | Monitor | Engage | Track |
| 01:00-06:00 | Monitor | Sleep | Triage | Respond | Sleep |
| 06:00 | Metrics | Standby | Report | Report | Report |
| 09:00 | Standup | Standup | Standup | Standup | Standup |
| 12:00-18:00 | Monitor | Fix (if needed) | Triage | Engage | Track |
| 18:00 | Metrics | Standby | Report | Report | Report |
| 23:59 | Post-mortem | Post-mortem | Post-mortem | Post-mortem | Post-mortem |

---

## 6. Age Ratings & Compliance

### 6.1 ESRB Rating (North America)

**Target Rating**: E (Everyone) or E10+ (Everyone 10+)

**Content Assessment**:
- No violence (management sim, no depictions of harm)
- No sexual content
- No strong language
- Mild thematic elements (corporate pressure, studio cancellations)
- Simulated gambling: None (no loot boxes, no randomized purchases)

**Expected Rating**: **E (Everyone)**

**ESRB Application Process**:
1. Submit application via ESRB portal: https://www.esrb.org/developers/
2. Provide gameplay footage (30-minute video showing typical gameplay loop)
3. Answer content questionnaire
4. Fee: ~$800 (for indie/small publisher)
5. Turnaround: 2-4 weeks

**Required Assets Post-Rating**:
- [ ] ESRB rating icon (E or E10+) for store pages
- [ ] ESRB content descriptors (if any, likely "Mild Themes")
- [ ] Display rating on packaging (if physical release)

---

### 6.2 IARC Rating (International, Digital Storefronts)

**What is IARC?**: International Age Rating Coalition — unified system for digital storefronts (Steam, Nintendo eShop, etc.)

**IARC Questionnaire**: Complete online at https://www.globalratings.com/
- Questions about violence, sexual content, language, gambling, user interaction
- Takes ~15 minutes
- Generates ratings for multiple regions automatically:
  - PEGI (Europe): Expect PEGI 3 or PEGI 7
  - USK (Germany): Expect 0 or 6
  - ClassInd (Brazil): Expect L (Livre)
  - ACB (Australia): Expect G (General)

**IARC Timeline**: Instant (questionnaire generates ratings immediately)

**Required Actions**:
- [ ] Complete IARC questionnaire
- [ ] Download rating certificates
- [ ] Upload rating icons to Steam store page
- [ ] Upload rating icons to Nintendo eShop page (if applicable)

---

### 6.3 Regional Compliance Notes

**Europe (PEGI)**:
- Expected rating: PEGI 3 (suitable for all ages)
- No content descriptors expected
- Display PEGI logo on store page and packaging

**Germany (USK)**:
- Expected rating: USK 0 (no age restriction)
- Steam automatically displays USK rating if IARC completed

**Australia (ACB)**:
- Expected rating: G (General)
- Australia is strict about content; management sim with no violence/sexual content is safe

**China**:
- Not launching in China initially (requires separate approval process, government review)
- Consider for post-launch expansion if demand exists

---

## 7. Post-Launch Monitoring Plan

### 7.1 Metrics to Track (Daily, Week 1)

**Sales & Revenue**
- [ ] Units sold (Steam stats)
- [ ] Revenue (Gross and net after platform fees)
- [ ] Refund rate (Target: <5%)
- [ ] Wishlists converted (Steam provides this data)

**Player Engagement**
- [ ] Concurrent players (peak and average)
- [ ] Average session length (Unity Analytics or Steam)
- [ ] Campaign completion rate (what % reach Year 10?)
- [ ] Drop-off points (which year do most players quit?)

**Reviews & Sentiment**
- [ ] Steam review score (Target: "Very Positive" = 80%+ positive)
- [ ] Total review count (more reviews = more visibility)
- [ ] Metacritic score (if review outlets cover the game)
- [ ] Social media sentiment (positive/negative/neutral mentions)

**Technical Health**
- [ ] Crash reports (Unity Crash Reporter or Sentry integration)
- [ ] Bug report volume (Steam forums, Discord)
- [ ] P0/P1 bugs outstanding

---

### 7.2 Monitoring Tools

**Steam Stats**
- Steamworks Partner Dashboard: https://partner.steamgames.com/
- Daily sales, concurrent players, regional breakdown
- Review scores and trends

**Unity Analytics** (Optional)
- Track player behavior: Which studios are most popular? Which interventions used most?
- Drop-off heatmaps: Where do players quit?
- Configuration: Unity Analytics SDK, opt-in during first launch

**Bug Tracking**
- Trello or Jira for bug triage
- Integrate with Discord (webhook for #bug-reports channel)

**Social Listening**
- TweetDeck or Hootsuite for Twitter/X monitoring
- Reddit alerts for r/Games, r/tycoon, r/gamedev mentions
- Steam forums daily review

---

### 7.3 Post-Launch Decision Framework

**Scenario: Positive Launch (80%+ Steam reviews, strong sales)**
- Action: Celebrate, but stay vigilant
- Focus: Community engagement, thank players, monitor for late-breaking bugs
- Next: Plan Patch 1.1 based on feedback (balance tuning, QoL)

**Scenario: Mixed Launch (60-79% Steam reviews, moderate sales)**
- Action: Triage common complaints
- Focus: Identify recurring pain points (balance too harsh? UI unclear? bugs?)
- Next: Prioritize hotfix for top 3 issues, communicate fix timeline

**Scenario: Negative Launch (<60% Steam reviews, low sales)**
- Action: Emergency triage
- Focus: Is there a critical bug affecting most players? Is the game fundamentally not fun?
- Next: If bug: Hotfix ASAP. If design: Deep retrospective, major patch planning

**Scenario: Critical Bug Discovered (Crash/corruption affecting >10% players)**
- Action: Activate rollback procedure (Section 4.1)
- Focus: Fix bug, deploy hotfix within 24-48 hours
- Next: Post-mortem — how did this slip through QA?

---

### 7.4 Community Management Protocol

**Daily Responsibilities** (Assigned to HYPE or Community Manager)
- [ ] Monitor Steam forums (respond to threads within 24 hours)
- [ ] Monitor Discord #general and #bug-reports (active presence)
- [ ] Monitor social media (@mentions, keyword searches)
- [ ] Respond to reviews (thank positive reviews, acknowledge negative ones professionally)

**Response Templates**

**Positive Review Response**:
```
Thank you for playing GREENLIGHT! We're thrilled you enjoyed the greenlight decisions and studio relationships. What was your publisher's legacy? We'd love to hear about your favorite studio arc.
```

**Negative Review (Bug-Related)**:
```
We're sorry you experienced [bug]. This is a known issue and we're working on a fix. ETA: [timeframe]. Your save is safe. Please contact us at [email] if you need further assistance.
```

**Negative Review (Design Complaint)**:
```
Thank you for the feedback. We hear your concerns about [mechanic]. We're collecting player feedback to inform balance tuning in the next patch. Your input helps us improve GREENLIGHT.
```

**Bug Report Response**:
```
Thank you for the detailed report. Can you provide the following to help us reproduce?
1. What year/quarter were you in?
2. Which studio/pitch was involved?
3. Your save file (attach if possible)

We'll investigate and update you soon.
```

---

### 7.5 When to Act (Decision Triggers)

**Trigger: >50 bug reports of same P0 issue within 24 hours**
- Action: Hotfix deployment within 48 hours

**Trigger: Steam review score drops below 70% positive**
- Action: Emergency team meeting, triage top complaints, communicate fix plan

**Trigger: Refund rate exceeds 10%**
- Action: Investigate common refund reasons (Steam provides this data), address root cause

**Trigger: Sales velocity drops >50% in Week 2 vs. Week 1**
- Action: Marketing push (coordinate with HYPE), consider launch discount extension

**Trigger: Players report "the game is too hard/unfair"**
- Action: Analyze balance data (board satisfaction thresholds, studio morale curves), plan balance patch

---

## 8. Patch 1.1 Planning (Month 2 Post-Launch)

### 8.1 Patch Goals

**Primary Goal**: Address player feedback from launch, improve quality of life, tune balance.

**Secondary Goal**: Re-engage lapsed players, drive positive review uptick.

---

### 8.2 Patch Content (Proposed)

**Bug Fixes**
- [ ] All P1 and P2 bugs from launch resolved
- [ ] Edge cases from player reports fixed

**Balance Tuning** (Data-Driven)
- [ ] Board targets adjusted based on player completion rates
  - If <40% reach Year 10: Reduce board pressure in Years 5-7
  - If >80% reach Year 10: Increase difficulty curve
- [ ] Intervention costs tuned based on usage data (most/least used interventions)
- [ ] Studio morale recovery rates adjusted if death spiral too common

**Quality of Life Features**
- [ ] Fast-forward button (skip quarter animations, accelerate turn execution)
- [ ] Undo last decision (limited to 1 undo per quarter, prevents misclick rage)
- [ ] Tooltips expanded (show exact calculation formulas on hover)
- [ ] Save slot expansion (10 → 20 slots if requested by players)
- [ ] Keybind customization (allow remapping of shortcuts)

**UI Polish**
- [ ] Color-blind mode (alternative palette for red/green UI elements)
- [ ] Font size options (accessibility for small-screen players)
- [ ] Notification history (log of all era events, studio emails)

---

### 8.3 Patch Development Timeline

**Week 1-2**: Feedback collection and prioritization
**Week 3-4**: Implementation and testing
**Week 5**: Beta testing (Steam beta branch, invite active players)
**Week 6**: Deploy Patch 1.1 to live

---

### 8.4 Patch Notes Template

```
GREENLIGHT — Patch 1.1 (Quality of Life Update)

Thank you to everyone who played GREENLIGHT and shared feedback. This patch addresses your most-requested features and balance concerns.

NEW FEATURES
- Fast-Forward Button: Skip quarter animations and speed up turn execution
- Undo Last Decision: Revert your last greenlight decision (once per quarter)
- Save Slot Expansion: 20 save slots (up from 10)

BALANCE ADJUSTMENTS
- Board Prestige targets reduced by 5-10 in Years 5-7 (based on completion data)
- "Expand Scope" intervention cost reduced from $15M to $12M
- Studio morale recovery rate increased from +5 to +7 per quarter (when no intervention applied)
- Auteur studio trust requirement for Magnum Opus reduced from 30 to 25

UI IMPROVEMENTS
- Color-blind mode added (Settings → Accessibility)
- Tooltips now show exact formulas for quality seed, revenue, portfolio impacts
- Notification history panel added (review past era events and studio emails)

BUG FIXES
- Fixed crash when canceling project in Q4 2008 with DLC mandate
- Fixed studio reputation cascade not applying correctly after lead designer firing
- Fixed save corruption when auto-saving during era event transition (rare edge case)
- Fixed Switch controller navigation getting stuck on portfolio matrix

We're committed to supporting GREENLIGHT post-launch. More updates coming soon.

— The GREENLIGHT Team
```

---

## 9. Launch Risks & Mitigation

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| **Critical bug slips through QA** | Medium | Critical | Clean machine testing, soak testing, beta test with 50+ external testers |
| **Save corruption post-launch** | Low | Critical | Atomic file writes, validation checks, auto-save rotation, QA save/load stress tests |
| **Poor review scores (< 70%)** | Medium | High | Balance validation via playtesting, pacing tuning, tutorial clarity |
| **Low sales (<1000 units Week 1)** | Medium | Medium | Marketing push (HYPE), press outreach, influencer keys, Steam visibility optimization |
| **Refund rate > 10%** | Low | Medium | Strong first hour (tutorial clarity), fair difficulty curve, save integrity |
| **Platform rejection (Switch)** | Low | Medium | Early devkit testing, Nintendo compliance review 6 weeks pre-launch |
| **Steam review bombing** | Low | High | Community management, respond professionally, address legitimate complaints quickly |
| **Server issues (Steam CDN)** | Very Low | Low | Valve handles infrastructure; no action needed (offline single-player game) |

---

## 10. Final Pre-Launch Checklist (T-Minus 1 Week)

**Build Validation**
- [ ] Gold master build tested on 3+ clean machines (Windows 10, Windows 11, various hardware)
- [ ] Save/load tested across 10+ full campaigns (no corruption)
- [ ] Performance targets met (60 FPS PC, 30 FPS Switch)
- [ ] No P0 or P1 bugs outstanding

**Store Pages**
- [ ] Steam store page live and accurate (pricing, description, assets)
- [ ] Nintendo eShop page live (if applicable)
- [ ] Age ratings displayed correctly

**Marketing**
- [ ] Launch trailer live on YouTube, embedded on Steam
- [ ] Press keys distributed (100+ sent, embargo lift date set)
- [ ] Influencer outreach complete
- [ ] Social media posts scheduled

**Team Readiness**
- [ ] Launch day timeline distributed
- [ ] Emergency contact sheet distributed
- [ ] Roles assigned (SHIP: deployment, CRASH: triage, HYPE: community, CLOCK: metrics)
- [ ] Day-one patch ready (if applicable)

**Rollback Plan**
- [ ] Last known good build identified and archived
- [ ] Rollback procedure documented and team-reviewed
- [ ] Steam rollback commands tested on beta branch

**Post-Launch Monitoring**
- [ ] Bug triage spreadsheet prepared
- [ ] Metrics dashboard configured (Steam stats, analytics)
- [ ] Community management protocol distributed

---

## 11. Post-Mortem Template (Fill Out 1 Week Post-Launch)

### What Went Right
- [List 3-5 things that went well]
- Example: "Clean machine testing caught a critical DLL dependency issue before gold"

### What Went Wrong
- [List 3-5 things that could have gone better]
- Example: "Board balance was too harsh in Years 6-7, caused mid-game drop-off"

### Metrics Summary
- Units sold (Week 1): [Number]
- Revenue (Week 1): [Number]
- Steam review score: [%]
- Metacritic score: [Number, if available]
- Peak concurrent players: [Number]
- Refund rate: [%]
- P0 bugs discovered post-launch: [Number]

### Action Items for Next Launch
- [List lessons learned, process improvements]
- Example: "Increase external beta test group to 100+ for better edge case coverage"

---

## 12. Emergency Contacts

**Platform Holders**
- Steam Support: https://partner.steamgames.com/support/
- Nintendo Developer Support: https://developer.nintendo.com/support

**Critical Team Members**
- SHIP (Release Manager): [Contact]
- BYTE (Lead Programmer): [Contact]
- CRASH (QA Lead): [Contact]
- HYPE (Marketing): [Contact]
- CLOCK (Producer): [Contact]

**Service Providers**
- PR Agency (if applicable): [Contact]
- Trailer Production (if external): [Contact]

---

## 13. Closing Notes

This release plan is a living document. It will be updated as we progress through development. But the core principles do not change:

1. **Test on clean machines.** If it doesn't work on a fresh install, it doesn't work.
2. **Plan for failure.** Every launch has a rollback scenario. Know yours before launch day.
3. **Communicate proactively.** Players forgive bugs if you acknowledge them and fix them fast.
4. **Monitor obsessively.** Launch day is not the end. It's the beginning of post-launch support.
5. **Ship when ready.** A delayed launch is a minor setback. A broken launch is permanent reputation damage.

The greenlight stamp comes down once. Make it count.

What's the rollback plan? Section 4.1.
When's the last commit before gold? Section 1.4, Code Freeze.
Who owns launch day deployment? SHIP (primary), BYTE (backup).

Is it on the checklist? If not, it doesn't happen.

Launch day should be boring. If nothing goes wrong, we did our job.

---

**Document Status**: Production-ready. Review with team 4 weeks pre-launch.

**Next Steps**: Begin Alpha playtesting (Week 21). Execute checklist sequentially.

— SHIP

---

## Appendix A: Build Artifact Checklist

**Per Platform, Per Build**:
- [ ] Executable (GREENLIGHT.exe or .nsp)
- [ ] Version number embedded and visible
- [ ] Build hash recorded
- [ ] Build date recorded
- [ ] Platform (PC x64, Switch ARM64)
- [ ] Configuration (Master/Release, not Debug)
- [ ] File size logged
- [ ] Archived in 3 locations (local, cloud, external drive)

**Gold Master Archive Contents**:
```
/GoldMaster_v1.0.0/
  /PC/
    GREENLIGHT.exe
    GREENLIGHT_Data/
    build_info.txt (version, hash, date)
  /Switch/ (if applicable)
    GREENLIGHT.nsp
    build_info.txt
  /Documentation/
    release-plan.md (this document)
    patch-notes.md
    known-issues.md
  /Marketing/
    trailer.mp4
    screenshots/ (all 10+)
    presskit.zip
```

---

## Appendix B: Common Launch Day Issues & Responses

**Issue: "Game won't launch (missing DLL)"**
- Cause: Missing Visual C++ Redistributable or .NET dependency
- Fix: Bundle redistributables with installer, or link to Microsoft download in error message
- Prevention: Test on clean machine without dev tools

**Issue: "Save files disappearing"**
- Cause: Save path incorrect, permissions issue, or antivirus blocking
- Fix: Add save path logging to Player.log, instruct player to whitelist game folder
- Prevention: Test save/load on multiple machines, various OS versions

**Issue: "Game crashes on Year X, Quarter Y"**
- Cause: Edge case in era event or studio AI logic
- Fix: Reproduce internally, deploy hotfix
- Prevention: Soak testing (automated playthrough to Year 10, 10+ runs)

**Issue: "Controller not working (Switch)"**
- Cause: Input mapping error, button conflicts
- Fix: Hotfix input configuration
- Prevention: Full controller QA pass on Switch devkit

**Issue: "Refund request: 'Too hard, unfair'"**
- Cause: Balance issue (board targets too aggressive)
- Fix: Patch 1.1 balance tuning
- Prevention: Playtesting with varied skill levels, difficulty curve validation

---

## Appendix C: Patch Deployment Checklist (Reusable)

**Pre-Deployment**
- [ ] Patch content finalized
- [ ] Version number incremented (e.g., 1.0.0 → 1.0.1)
- [ ] Patch notes written
- [ ] QA regression testing complete (no new bugs introduced)
- [ ] Beta testing on Steam beta branch (if major patch)

**Deployment**
- [ ] Upload patch build to Steam depot
- [ ] Set live on Steam (or schedule for off-peak hours)
- [ ] Upload patch to Nintendo (if applicable, factor 2-4 week approval)
- [ ] Post patch notes in Steam community hub
- [ ] Post patch notes in Discord #announcements
- [ ] Post patch notes on social media

**Post-Deployment**
- [ ] Verify patch live (download with test account)
- [ ] Monitor for new bug reports (first 24 hours critical)
- [ ] Track player sentiment (reviews, social media)
- [ ] Update known issues list if patch doesn't fully resolve issue

---

**End of Release Plan.**

**This document is the last line of defense between GREENLIGHT and the player.**

**If it's not on the checklist, it doesn't happen.**

**What's the rollback plan? You know where to find it.**

— SHIP
