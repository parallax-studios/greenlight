# GREENLIGHT — Sound Design Document

**Sound Designer / Composer**: ECHO
**Version**: 1.0
**Date**: 2026-02-07
**Status**: Pre-Production

---

> "Close your eyes. What do you hear when someone chooses which dreams live and which die?"
> -- ECHO

---

## 1. Sonic Identity Statement

GREENLIGHT is the sound of institutional power made intimate. The hum of fluorescent lights in an empty boardroom at 7 PM. The satisfying mechanical THUNK of a rubber stamp meeting paper. The projector fan that whirs while someone's career hangs in the air. The keyboard clicks of emails that end with "we've decided to move in a different direction."

This is corporate ambience rendered with the precision of a horror film foley pass — because the horror here isn't monsters, it's being the person who signs the cancellation notice. Every sound must carry the texture of bureaucracy, the weight of consequence, and the ghost of the human lives behind the data.

Audio IS game feel here. The stamp coming down on a greenlight decision is not just confirmation feedback. It's the entire game condensed into 1.2 seconds of sound design. If that stamp doesn't feel satisfying, weighty, and slightly ominous, the game's emotional core collapses.

**The sonic thesis**: Power sounds different when you're the one wielding it. Every interface click, every notification ping, every milestone completion — they're not UI sounds. They're the architecture of a publisher's internal experience. Players don't just hear feedback. They hear themselves becoming the institution.

---

## 2. Sonic Palette Description

### Primary Tone: Corporate Tactile

The world of GREENLIGHT exists in conference rooms, executive suites, and email inboxes circa 2005. The sonic palette mirrors this: physical office machinery meets digital sterility.

**Texture**: Mechanical, analog, institutional. Think metal desk accessories, leather office chairs, paper documents sliding across mahogany tables. The era is pre-iPhone, pre-cloud — this is the last gasp of physical office culture before everything became glass and aluminum. That physicality must be audible.

**Frequency Profile**: Mid-heavy with deliberate low-end weight. The stamp mechanism lives in the 200-600 Hz range (presence, authority). Fluorescent hum sits at 120 Hz (subliminal institutional drone). UI clicks are crisp but never shrill — centered around 2-4 kHz with minimal high-frequency sizzle. This is not a bright, optimistic soundscape. It's grounded, weighty, serious.

**Dynamic Range**: Controlled but expressive. The game has quiet moments (reading a pitch alone in your office) and loud moments (E3 showfloor, board confrontations). The mix must breathe. Silence is a compositional tool, not dead air.

**Emotional Register**: Contemplative unease. There's beauty in the bureaucracy, but it's a cold beauty. Think the sonic aesthetic of *Severance* (TV) meets the tactile UI sounds of *Papers, Please* meets the institutional ambience of *The Conversation* (film, 1974).

### Secondary Tone: Human Ghosting

Beneath the corporate layer, human presence bleeds through. Studio representatives breathe before pitching. Lead designers type frantically when morale is low. A coffee mug scrapes against a conference table during a tense negotiation. These sounds are sparse, borderline subliminal — but they remind the player that spreadsheets represent people.

**Implementation**: Layered as randomized micro-events. When reviewing a pitch from an Auteur studio, a distant sound of someone pacing might play at -18 dB. When a Volatile studio's morale crashes, the project card UI might emit a faint sound of papers being shuffled anxiously. Never obvious. Always felt.

### Tertiary Tone: Era-Specific Technological Nostalgia

The game spans 2003-2012. Technology sounds must date correctly. Early years: CRT monitor hum, dial-up handshake echoes in distant server rooms, Windows XP UI sounds. Mid-years: LCD hum (different frequency than CRT), early smartphone notification tones beginning to intrude. Late years: the quiet buzz of solid-state everything, the modern hum.

**Do not**: Use generic "retro" 8-bit bleeps as nostalgia signifiers. That's the wrong nostalgia. This is office nostalgia. Bloomberg Terminal nostalgia. Excel spreadsheet nostalgia. The sounds of institutional decision-making in the mid-2000s.

---

### Reference Tracks

**For Corporate Ambience**:
- *Severance* (Apple TV+, 2022) — Ben Stiller, dir. — The hum of institutions. Cold, precise, beautiful in its inhumanity.
- *The Social Network* (2010, Trent Reznor & Atticus Ross) — "Hand Covers Bruise" — Minimal piano over digital hum. Ambition rendered as texture.
- *Anatomy of a Fall* (2023) — Sound design by Emmanuelle Villard — Forensic listening. Every sound is evidence.

**For Mechanical UI / Tactile Feedback**:
- *Papers, Please* (Lucas Pope, 2013) — The stamp mechanism. The passport thunk. The physical satisfaction of bureaucracy.
- *Return of the Obra Dinn* (Lucas Pope, 2018) — UI sounds as period-accurate mechanisms. Clicks that feel like actual switches.
- *Inscryption* (Daniel Mullins, 2021) — Cards hitting wood. Physical game objects rendered in audio.

**For Subtle Emotional Tension**:
- *There Will Be Blood* (2007, Jonny Greenwood) — Strings used as texture, not melody. Unease as composition.
- *The Assassination of Jesse James...* (2007, Nick Cave & Warren Ellis) — Sparse, haunting, patient. Silence used aggressively.
- *Chernobyl* (HBO, 2019, Hildur Guðnadóttir) — Institutional horror. The sound of systems failing and people dying inside them.

**For Adaptive Music Systems**:
- *Stellaris* (Paradox, Andreas Waldetoft) — Music that responds to empire state without calling attention to itself.
- *Hitman* (2016, Niels Bye Nielsen) — Tension layers that add/subtract based on gameplay state.
- *Civilization VI* (Geoff Knorr, Christopher Tin) — Era-aware music that evolves as the campaign progresses.

---

## 3. SFX List by Category

### 3.1 Core Interaction — The Greenlight Table

This is the atomic unit of the game. These sounds must be perfect.

**Stamp Mechanism — Greenlight Approval**
- **Sound**: Heavy rubber stamp hitting paper over a solid desk. 1.2-second event.
  - **Layer 1 (Impact)**: Rubber on paper, 800 Hz fundamental with 1.6 kHz harmonic. Sharp attack, 40ms decay. Recorded with Contact Mic on actual rubber stamp for authenticity.
  - **Layer 2 (Mechanical)**: Spring-loaded mechanism, metal-on-metal click at tail. 2.4 kHz transient.
  - **Layer 3 (Resonance)**: Desk resonance, low-end thump at 180 Hz, 600ms decay. Gives institutional weight.
  - **Layer 4 (Paper Settle)**: Paper flutter as document settles, 200ms tail, high-frequency rustle (6-8 kHz) at -12 dB relative to impact.
- **Variants**: Three recordings with slight pitch/timing variance (±8%) to avoid fatigue on repeat stamping.
- **Implementation**: Unity AudioSource with random selection from variant pool. Light compression (3:1 ratio) to ensure punch even when multiple UI sounds overlap.
- **Feel Target**: The single most satisfying sound in the game. Players should want to stamp just to hear it. Reference: *Papers, Please* approval stamp + *Resident Evil* typewriter save.

**Stamp Mechanism — Pass (Red X)**
- **Sound**: Rejection stamp with harsher transient, slightly higher pitch (rejection sounds "thinner").
  - **Layer 1**: Same base stamp but pitch-shifted +6%, reduces weight. Feels dismissive.
  - **Layer 2**: Metal X-stamper mechanism, sharper click, 3.2 kHz.
  - **Layer 3**: No desk resonance layer. Rejection has less gravity. Intentionally less satisfying.
  - **Layer 4**: Paper slide-away sound (document rejected, pushed off-screen), 400ms swoosh with Doppler shift downward.
- **Feel Target**: Slightly uncomfortable. The player should *prefer* to greenlight based on sound alone. Make "no" feel worse.

**Pitch Dossier — Page Turn / Document Slide**
- **Sound**: Paper sliding across mahogany table, 600-800ms.
  - Recorded foley: actual resume paper (heavier stock) on wood desk.
  - Pitch variance ±5% per trigger to simulate natural hand movement.
  - EQ: Roll-off below 150 Hz (no mud), gentle boost at 1.5 kHz (presence).
- **Trigger**: When advancing between pitch dossier pages or opening a new pitch.

**Counter-Offer Negotiation — Acceptance**
- **Sound**: Hesitant agreement. Pen signing paper (scratchy nib), 1.2 seconds.
  - Fountain pen on heavy paper stock, recorded at close distance.
  - Slight hand tremor audible in stroke (studio is uncertain but accepting).
  - Tail: Document slide into "accepted" pile.
- **Variants**: Two versions (confident signature vs. reluctant signature) chosen based on studio Trust level.

**Counter-Offer Negotiation — Rejection**
- **Sound**: Papers pushed back across table, 800ms.
  - Sharp, dismissive. No pen sound (unsigned).
  - Slight paper crumple (frustration).
  - Tail: Studio representative standing up from chair (leather chair creak, distant).
- **Implementation**: Triggered when counter-offer rejected. Morale drop notification immediately follows.

**Pitch Card Hover — Subtle Paper Lift**
- **Sound**: Barely audible paper lift, 150ms.
  - High-frequency rustle at -18 dB. Felt more than heard.
  - Communicates "this document can be interacted with" without being obtrusive.
- **Trigger**: Mouse hover over pitch card in Greenlight Table interface.

**Budget Ticker — Funds Decrementing**
- **Sound**: Mechanical counter, money changer, old-school register.
  - Short, percussive. Metal cylinder rotation, 200ms per decrement.
  - Pitch descends slightly with each million deducted (psychological: losing money sounds "lower").
  - Reference: Pinball machine score counter, analog cash register bell (but subdued).
- **Trigger**: When greenlight approved and budget decrements visibly in sidebar.

---

### 3.2 Milestone Review & Intervention System

**Project Card — Status Update Appear**
- **Sound**: Digital notification meets paper arrival.
  - Soft email "whoosh" (2003-era Outlook receive sound aesthetic) + paper drop on desk.
  - 600ms, pitched around C4 (neutral, informational).
  - Subtle low-end thump if status is "Crisis" (morale < 30 or budget overrun > 30%).
- **Trigger**: When milestone review packages arrive in management phase.

**Morale Bar — Fill/Drain Animation**
- **Sound**: Liquid-like UI element with emotional color.
  - **Rising Morale**: Glass fill, bright resonance, ascending pitch (E4 to G4), 800ms. Optimistic.
  - **Falling Morale**: Glass drain, descending pitch (E4 to C3), 1.2 seconds. Ends with slight hollow thunk (emptiness).
  - Not realistic liquid foley — stylized, UI-appropriate, but emotionally readable.
- **Trigger**: When intervention or restraint changes studio morale. Animated sync with visual bar.

**Intervention Selection — Decision Confirmation**
- **Sound**: Mouse click meets mechanical selector switch.
  - Sharp click (1-2 kHz transient) + subtle relay switch (400 Hz, mechanical).
  - Each intervention type has slight pitch variance:
    - Aggressive interventions (Force Multiplayer, Ship Now): Harsher click, +10% pitch.
    - Supportive interventions (Expand Scope, Delay to Polish): Softer click, -5% pitch.
    - Destructive interventions (Cancel Project, Fire Designer): Metallic, cold, +15% pitch with longer reverb tail (finality).
- **Trigger**: When player confirms intervention choice.

**Restraint — "Do Nothing" Selection**
- **Sound**: Absence emphasized. Negative space made deliberate.
  - Very quiet room tone swell, 1 second fade-in.
  - Distant office ambience (muffled, like stepping out of a decision).
  - A single exhale (barely audible, player character letting go of control).
- **Feel Target**: Must feel like a powerful choice, not inaction. Silence as statement.

**Crisis Alert — Project in Trouble**
- **Sound**: Urgent but not panicked. Institutional alarm.
  - Low-frequency pulse, 2 Hz (heartbeat-adjacent). 250 Hz sine + 500 Hz harmonic.
  - Gentle rise in volume over 2 seconds, never exceeds -12 dB (not a jump scare).
  - Tail: Office phone ringing distantly (old landline ring, muffled through walls).
- **Trigger**: When project card enters crisis state (morale < 20, budget overrun > 40%).

**Cancellation Notice — Project Killed**
- **Sound**: The worst sound in the game. Institutional execution.
  - Three-stage event, 4 seconds total:
    - **Stage 1 (0.0s)**: Red stamp (harsher than rejection stamp, metallic).
    - **Stage 2 (1.2s)**: Papers swept into trash, physical and dismissive.
    - **Stage 3 (2.5s)**: Project card UI element collapses with pitched-down "close" sound (descending from D4 to A2, 1.5 seconds). Death rattle of a game.
  - Tail: Silence for 1 second before next sound event. Let it breathe.
- **Feel Target**: Player should feel this. Not punishing, but mournful. A dream just died.

---

### 3.3 Portfolio Matrix & Board Interactions

**Portfolio Triangle — Axis Shift**
- **Sound**: Geometric UI element with synthesized tone.
  - Three-note chord (Prestige = E, Profit = A, Populism = C) where each axis is a sine wave voice.
  - When axis value rises: Note brightens (filter opens, +6 dB boost at axis-specific frequency).
  - When axis value falls: Note darkens (filter closes, -8 dB at frequency).
  - Result: The Portfolio Matrix hums a chord that tells you its health without looking.
- **Trigger**: After each shipped game modifies the matrix. Real-time sonification.

**Board Evaluation — Annual Review Transition**
- **Sound**: Transition into formal presentation mode.
  - Room tone shift: Office ambience fades, replaced by boardroom ambience (larger space, marble floors, higher ceilings).
  - Projector warm-up (2-second mechanical whir, CRT-era projector bulb ignition).
  - Leather chair creaks as board members settle.
  - A single cleared throat (distant, authoritative).
- **Duration**: 5 seconds. Sets stage for board evaluation scene.

**Board Member — Voice Processing**
- **Sound**: Dialogue (if voice-acted) or abstract vocal texture (if text-only) with specific processing.
  - Light room reverb (boardroom acoustic: 1.8s RT60, early reflections at 40ms).
  - Subtle EQ: Roll-off below 120 Hz (removes intimacy, increases authority).
  - Compression: 4:1 ratio (controlled, never emotional).
  - Each board member has distinct pitch/timbre:
    - CFO: Lower register, clipped delivery.
    - Creative Officer: Mid-register, slightly warmer.
    - Chairperson: Authoritative, centered, neutral.
- **Implementation**: If text-only UI, board member portraits could emit pitched hums (barely audible) corresponding to their voice archetypes. Subliminal characterization.

**Board Satisfaction — Positive Result**
- **Sound**: Approval without warmth. Professional validation.
  - Polite golf clap (3 seconds, recorded at distance, reverberant).
  - Light briefcase clasps (business concluding).
  - Ascending three-note motif (major triad: C-E-G, orchestral brass, 2 seconds).
- **Trigger**: When Board Satisfaction > 110 at annual review.

**Board Satisfaction — Warning Issued**
- **Sound**: Institutional tension.
  - Papers shuffled anxiously.
  - A pen clicking (nervous tic).
  - Descending two-note motif (minor second: E-Eb, strings, dissonant, 3 seconds).
  - Long silence afterward (the room waiting for your response).
- **Trigger**: When Board Satisfaction 70-89 (warning threshold).

**Board Satisfaction — Termination Risk**
- **Sound**: Crisis without hysteria. Cold institutional threat.
  - Boardroom ambience becomes oppressive (reverb tail extends to 3 seconds, claustrophobic).
  - Low-frequency drone (80 Hz, slow LFO wobble, 0.2 Hz).
  - No music. Just room tone and the sound of your own failure.
- **Trigger**: When Board Satisfaction < 50 (termination review).

---

### 3.4 Studio Relationship & Communication

**Email Arrival — Studio Correspondence**
- **Sound**: 2005-era email notification aesthetic.
  - Outlook-style chime (mid-frequency bell, C5, 400ms decay).
  - Variants by studio archetype:
    - **Auteur**: Slightly detuned, +3% pitch (precious, artistic).
    - **Reliable**: On-pitch, clean (professional).
    - **Volatile**: Random pitch variance ±8% (unstable).
    - **Fading**: Slightly muffled, -5% pitch (tired, nostalgic).
- **Trigger**: When studio sends email (pitch, thank-you, complaint, crisis alert).

**Trust Change — Relationship Shift**
- **Sound**: Interpersonal indicator rendered abstractly.
  - **Trust Rising**: Warm analog synth pad, major chord swell (C major, 2 seconds, +3 dB/sec slope).
  - **Trust Falling**: Cold analog synth pad, minor chord descent (C minor to Ab, 2 seconds, -3 dB/sec slope).
  - Processing: Tape saturation (warmth for rising, tape dropout artifacts for falling).
- **Trigger**: When Trust value changes ±5 or more.

**Studio Pitch Presentation — Slide Advance**
- **Sound**: PowerPoint slide change (circa 2005).
  - Soft "whoosh" + projector focus adjustment (mechanical lens motor, 300ms).
  - Each studio archetype has distinct presentation audio:
    - **Auteur**: Hand-crafted slides, paper texture in transitions, occasional mic feedback (passion bleeding through tech).
    - **Reliable**: Polished corporate template, clean transitions, no artifacts (professional, sterile).
    - **Volatile**: Erratic pacing, slides load slowly (technical difficulties), occasional glitch sounds (chaos energy).
    - **Fading**: Slides are scanned images of old press kits, scan line artifacts audible, nostalgic tape hiss (living in the past).
- **Trigger**: When advancing through studio pitch deck in Greenlight Table.

**Studio Morale — Background State Layer**
- **Sound**: Ambient audio layer that plays subtly when viewing a studio's project card, reflecting morale state.
  - **High Morale (80-100)**: Faint typing sounds (productive, rhythmic), distant conversation (team collaboration), coffee machine gurgle (comfortable).
  - **Medium Morale (40-79)**: Neutral office ambience, minimal character.
  - **Low Morale (20-39)**: Slow typing (demoralized), long silences, occasional stressed sigh, distant argument (muffled).
  - **Crisis Morale (0-19)**: Almost silent. Hollow room tone. A phone ringing unanswered. Abandonment.
- **Implementation**: Crossfade based on morale value. Plays at -20 dB, barely conscious, felt as mood.

---

### 3.5 Era Engine & Historical Events

**Era Transition — New Year Begins**
- **Sound**: Calendar year rollover meets historical shift.
  - Desk calendar page tear (physical, satisfying).
  - Distant news broadcast (muffled radio/TV, era-appropriate tech).
  - Subtle music shift (composition changes to reflect new era's tone — see Music section).
- **Duration**: 3 seconds.

**Era Event Notification — Industry Shift**
- **Sound**: Breaking news meets systemic alert.
  - News broadcast sting (2000s TV news aesthetic: urgent but polished).
  - Voiceover (processed): "Industry sources report..." [if voice-acted] or synthesized vocal texture [if abstract].
  - Underlying sound: Market ticker (financial data stream, percussive, fast).
- **Variants**:
  - **Positive Industry Shifts** (new platform launch, market expansion): Major key sting, ascending.
  - **Disruptive Shifts** (recession, format war): Minor key sting, tense.
  - **Neutral Shifts** (Metacritic influence grows): Atonal, informational.
- **Trigger**: When major Era Event fires (console launch, DLC emergence, recession, etc.).

**Console Launch Announcement**
- **Sound**: Hype meets anticipation. E3 press conference aesthetic.
  - Crowd murmur (convention center ambience).
  - Distant applause.
  - Trailer music sting (bombastic, 2000s E3 energy: orchestral brass + electronic pulse).
- **Examples**:
  - **Xbox 360 (2005)**: Metallic, futuristic, cold green aesthetic.
  - **Wii (2006)**: Bright, casual, major key, family-friendly energy.
  - **PS3 (2006)**: Prestige, cinematic, orchestral weight.
- **Duration**: 4 seconds per announcement.

---

### 3.6 Game Shipment & Review Reception

**Game Ships — Gold Master**
- **Sound**: Completion and release.
  - CD burner finalizing disc (2000s-era burning software completion sound).
  - Printer finishing (cover art printed).
  - Box sealing (physical media packaging, tape gun, satisfying closure).
  - Tail: Distant truck loading (shipping, distribution, game entering the world).
- **Duration**: 5 seconds. Feels like ceremony.

**Metacritic Score Reveal**
- **Sound**: The number that haunts you.
  - Newspaper printing press (scores going live, old media aesthetic).
  - Review aggregation UI element: Digital counter ticking upward to final score (mechanical, tense, 2 seconds).
  - **Score-Dependent Tail**:
    - **90-100**: Triumphant strings, major key, 3 seconds. Award show energy.
    - **80-89**: Warm approval, subtle pad, 2 seconds. Solid success.
    - **70-79**: Neutral tone, no emotional color. Competent.
    - **60-69**: Minor key, slightly disappointing, 2 seconds.
    - **< 60**: Cold, descending tone, 3 seconds. Failure rendered as audio.
- **Trigger**: When game reviews arrive post-ship.

**Sales Report — Revenue Tally**
- **Sound**: Financial results rendered as satisfying counting.
  - Cash register bell (vintage, mechanical).
  - Money counter machine (bills being counted, rhythmic).
  - If profit: Ascending arpeggio (major scale, bright).
  - If loss: Descending tone (minor, subdued).
- **Duration**: 3 seconds.

**Franchise Success — Sequel Opportunity**
- **Sound**: IP value crystallizing.
  - Slot machine jackpot aesthetic (but tasteful, not gaudy).
  - Three-note ascending fanfare (franchise theme established).
  - Cash register cha-ching (this IP is now a revenue stream).
- **Trigger**: When shipped game scores 80+ and generates sequel pitch opportunity.

---

### 3.7 UI & Navigation

**Menu Navigation — Hover**
- **Sound**: Soft UI highlight.
  - Subtle synth tone, 120ms, C5.
  - No attack transient (smooth, not clicky).
  - Gentle filter sweep (opens on hover).
- **Trigger**: Mouse hover over interactive UI element.

**Menu Navigation — Click**
- **Sound**: Interface confirmation.
  - Clean click, 80ms, pitched at D5.
  - Slight analog warmth (not digital/harsh).
  - Variants by UI context:
    - Primary actions (Greenlight, Intervene): Fuller click, +3 dB.
    - Secondary actions (View Details, Cancel): Lighter click, -2 dB.
    - Destructive actions (Fire Designer, Cancel Project): Harsher click, metallic.
- **Trigger**: Mouse click on button/interactive element.

**Tab Switch — Interface Panel Change**
- **Sound**: Paper folder change or filing cabinet drawer.
  - 400ms, physical foley (actual office furniture).
  - Subtle air movement (folder opened, papers rustle).
- **Trigger**: Switching between Greenlight Table, Milestone Reviews, Portfolio Matrix, Studio Relationships.

**Loading Transition — Between Quarters**
- **Sound**: Time passing institutionally.
  - Clock ticking (wall clock, office acoustic, 4 ticks).
  - Calendar page turn.
  - Distant office ambience fade-in (new quarter begins).
- **Duration**: 2-3 seconds.

**Pause Menu — Game Paused**
- **Sound**: Time suspension.
  - All game audio lowpass-filtered (cutoff at 800 Hz, resonance 0.3).
  - Room tone continues but muffled.
  - Clock stops ticking.
- **Resume**: Filter opens over 400ms, time resumes.

---

### 3.8 Endgame & Special Events

**Termination — Fired by Board**
- **Sound**: Institutional execution, personal devastation.
  - Gavel strike (single, final, authoritative).
  - Papers swept off desk (your tenure, discarded).
  - Office door closing (heavy, final, distant reverb).
  - Footsteps walking away down empty hallway (you, leaving).
  - Fade to silence over 8 seconds.
- **No music.** Let the silence communicate loss.

**Legacy Montage — Campaign Complete**
- **Sound**: Memory and retrospective.
  - Slideshow projector click-advance (Kodak Carousel aesthetic, 1 click per game/decision shown).
  - Layered studio voices (abstracted, pitched, processed as texture — the people you worked with).
  - Underlying pad: Tape hiss + analog warmth (nostalgia, distance, time passed).
  - Final moment: Office lights turning off (switch click, fluorescent fade-out, 3 seconds).
- **Music**: Full orchestral piece (see Music section). The only time the score is foregrounded.

**Award Win Notification — Prestige Achievement**
- **Sound**: Industry recognition.
  - Award show applause (formal, sustained, 4 seconds).
  - Trophy pedestal placement (metal on marble, ringing resonance).
  - Camera flash bulbs (old-school press photography, rapid clicks).
- **Trigger**: When published game wins major industry award (unlocks based on Metacritic + Prestige combo).

**Studio Departure — Relationship Destroyed**
- **Sound**: Burned bridge.
  - Email deletion sound (trash bin, finality).
  - Phone line disconnecting (dial tone, then dead air).
  - Tail: Distant door slam (they've left the building).
- **Trigger**: When studio Trust drops below -30 and they leave your roster.

**Studio Magnum Opus Offer — Exclusive Pitch**
- **Sound**: Once-in-a-career opportunity.
  - Handwritten letter unfolding (paper texture, personal, intimate).
  - Wax seal breaking (old-world, significant).
  - Underlying: Orchestral swell (potential, grandeur, 4 seconds).
- **Trigger**: When Auteur studio with Trust > 30 offers their magnum opus pitch.

---

## 4. Music Brief

### Style & Tone

The score for GREENLIGHT is not entertainment. It is **functional ambience with emotional intelligence**. Music must support contemplation, enhance tension, and disappear when it should. This is not a game that needs a memorable melody. It needs a sonic environment that makes decisions feel weightier.

**Instrumentation Core**:
- **Strings**: Cello and double bass (institutional weight, low-end authority). Viola for mid-range tension.
- **Piano**: Prepared piano and felt-muted piano (mechanical, percussive, emotionally neutral).
- **Analog Synthesizers**: Moog-style bass (sub-frequency authority), Juno-style pads (cold corporate warmth).
- **Ambient Texture**: Tape loops, vinyl crackle, electromagnetic interference recordings (office building hum, server room drone).
- **Sparse Brass**: French horn (for moments of gravitas), muted trumpet (for era-specific flourishes in E3 sequences).

**What's NOT in the score**:
- Guitars (wrong energy, too personal).
- Drums/Percussion (except prepared piano hammers and industrial metal samples for crisis moments).
- Melodic hooks (this isn't a game players hum afterward — it's a game they *feel*).
- Uplifting major-key resolutions (optimism is earned, not given).

### Emotional Arc Across Campaign

**Years 1-3 (Learning Phase)**:
- **Mood**: Curiosity, possibility, controlled optimism.
- **Musical Palette**: Minimal piano motifs (3-4 note figures, spacious, Patient). Analog synth pads in major/suspended chords. Low string drones (not ominous, just present).
- **Tempo**: Slow (60-72 BPM). The player is learning. Music should not rush them.
- **Reference**: Brian Eno's *Music for Airports* meets Trent Reznor's *The Social Network* score. Ambient but purposeful.

**Years 4-6 (Disruption Phase)**:
- **Mood**: Complexity, mounting pressure, strategic focus.
- **Musical Palette**: Layered strings (counterpoint between cello and viola, creating harmonic tension). Prepared piano becomes more percussive. Synth pads introduce dissonance (minor seconds, tritones).
- **Tempo**: Moderate (80-92 BPM). The game is moving faster. Music reflects escalation.
- **Adaptive Element**: As board targets tighten, a subtle low-frequency pulse (80 Hz sine, 1 Hz LFO) begins appearing in the mix. Heartbeat-adjacent. Barely conscious.
- **Reference**: Hildur Guðnadóttir's *Chernobyl* score (systems under pressure). Jóhann Jóhannsson's *Arrival* (intellectual tension).

**Years 7-8 (Specialization Phase)**:
- **Mood**: Focus, commitment, the long game maturing.
- **Musical Palette**: Full string section (quartet + bass), playing sustained chords with slow harmonic motion (one chord change per 8-12 seconds). Analog synth arpeggios (slow, deliberate, Tangerine Dream-adjacent). Tape loops with degraded audio (time passing, legacy accumulating).
- **Tempo**: Variable (60-100 BPM depending on player's board standing). Music becomes more reactive.
- **Adaptive Element**: If Portfolio Matrix is balanced (all axes within 10 points of each other), music is consonant (major/suspended harmonies). If unbalanced, music becomes increasingly dissonant (cluster chords, microtonal detuning).
- **Reference**: Max Richter's *Sleep* (patient, evolving). Ben Frost's *By the Throat* (cold, beautiful, relentless).

**Years 9-10 (Legacy Phase)**:
- **Mood**: Culmination, reflection, the weight of ten years.
- **Musical Palette**: All previous elements layered. Strings play long tones (whole notes, 4-8 second sustains). Piano plays single-note melody (first melodic content in the entire score — earned, not given). Analog synths provide harmonic bed. Tape hiss and vinyl crackle become prominent (nostalgia, distance).
- **Tempo**: Slow (55-65 BPM). The game is asking the player to reflect, not react.
- **Adaptive Element**: The score subtly quotes earlier musical motifs from Years 1-3, but processed through time (pitchshifted down, reversed, time-stretched). The player is hearing echoes of their earlier self.
- **Reference**: Stars of the Lid (glacial, overwhelming, beautiful). William Basinski's *The Disintegration Loops* (decay as composition).

### Adaptive Music System Design

GREENLIGHT uses a **layered stem system** where musical intensity scales with game state, combined with **harmonic color shifts** based on portfolio health.

#### Layer Structure (Per Musical Cue)

Each music cue consists of 4-6 stems that can be enabled/disabled in real-time:

**Example: "Mid-Game Strategic Theme"**
- **Stem 1 (Foundation)**: Cello drone, sustained low C (65 Hz), sempre. Always playing.
- **Stem 2 (Harmony)**: Analog synth pad, slow chord progression (C minor to Ab major to Bb suspended). Plays when player is in Greenlight Table or Milestone Review.
- **Stem 3 (Tension)**: Viola counterpoint, dissonant intervals, sparse. Plays when Board Satisfaction < 90.
- **Stem 4 (Urgency)**: Prepared piano, rhythmic 8th notes, percussive. Plays when active project in crisis (morale < 30 or budget overrun > 30%).
- **Stem 5 (Resolution)**: Double bass pizzicato, slow melodic descent. Plays after successful game ships (Metacritic > 75).
- **Stem 6 (Crisis)**: String cluster chord, dissonant, sustained. Plays when Board Satisfaction < 70.

**Implementation**: Each stem is a looped audio file (1-2 minute loop length to avoid short-loop fatigue). Middleware (FMOD or Wwise) enables/disables stems with 2-second crossfades based on game state parameters:
- `BoardSatisfaction` (0-150)
- `ActiveProjectCrisisCount` (0-6)
- `PlayerIsInCriticalDecision` (bool)
- `GamePhase` (1-10, corresponding to years)

#### Harmonic Color System

The underlying **key center** of the music shifts based on Portfolio Matrix state:

| Portfolio State | Key Center | Mood |
|---|---|---|
| Balanced (all axes within 10 pts) | C Major / A Minor | Neutral, focused |
| Prestige-dominant (P > 70) | E Minor | Contemplative, artistic |
| Profit-dominant (P > 70) | G Major | Confident, corporate |
| Populism-dominant (P > 70) | D Major | Bright, accessible |
| Crisis (any axis < 40) | Atonal / Microtonal | Unstable, anxious |

**Implementation**: Stems are recorded in C (neutral). Middleware pitchshifts stems in real-time (±4 semitones max) to match target key center. Transitions happen over 8-12 seconds (one full chord progression) to avoid jarring shifts.

#### Silence as Adaptive Element

**Intentional Silence Triggers**:
- After canceling a project: 8 seconds of silence before music resumes.
- During Restraint decision: Music crossfades to room tone only.
- In Termination scene: No music until epilogue montage.
- First 30 seconds of each new fiscal year: Silence, then music fades in over 15 seconds.

Music absence is as important as presence. The player must have space to think, to feel the weight of decisions without a score telling them how to feel.

---

### Specific Musical Cues (Authored Pieces)

**Opening Theme — "The Inherited Cancellation"**
- **Duration**: 3 minutes (plays during opening scenario).
- **Structure**: Solo cello (low register, sul tasto for darkness), playing a 4-note descending motif (E-D-C-B). Each note sustained for 8 seconds. No other instrumentation. Room reverb (3-second tail, cold).
- **Emotional Target**: You are alone. You are about to kill someone's dream. There is no right answer.
- **Reference**: The opening of *There Will Be Blood* (strings as omen).

**E3 Showcase Theme — "The Showfloor"**
- **Duration**: 2 minutes (loops during E3 event).
- **Structure**: Bombastic, ironic. Full orchestral brass (4 French horns, 3 trumpets, 2 trombones) playing a triumphant fanfare in Bb Major. Underneath: a cold analog synth bassline (slightly out of sync with the brass, creating rhythmic tension). The fanfare is sincere; the bassline reminds you it's all a show.
- **Emotional Target**: Spectacle, hype, the industry's Super Bowl — but you're watching it as a strategist, not a fan.
- **Reference**: E3 2006 press conference music. Halo theme bombast meets Reznor's cynicism.

**Annual Review Theme — "The Board Awaits"**
- **Duration**: 90 seconds (plays during board evaluation scene).
- **Structure**: String quartet (2 violins, viola, cello) playing slow, tense counterpoint. No melody — only harmony. Dissonant suspensions resolve reluctantly. Ends on an unresolved dominant chord (leaves player hanging).
- **Emotional Target**: Judgment. Waiting for a verdict. The room is watching you.
- **Reference**: Shostakovich's String Quartet No. 8 (tension, institutional pressure).

**Legacy Montage Theme — "What You Built"**
- **Duration**: 5 minutes (plays during final campaign montage).
- **Structure**: Full orchestra (small chamber ensemble: 8 strings, 2 French horns, piano, analog synth pad). Begins with solo piano (the 4-note motif from Opening Theme, now in major key, transformed). Strings enter gradually, building to a sustained chord (Cmaj9, 45 seconds). Piano plays a simple, earned melody over the top. No climax. Just arrival.
- **Emotional Target**: Not triumph. Not regret. Reflection. You did something. It mattered. It's over.
- **Reference**: Max Richter's *On the Nature of Daylight* (emotional without manipulation).

**Termination Theme — "Exit Interview"**
- **Duration**: 2 minutes (plays during Termination Epilogue).
- **Structure**: Solo piano (felt-muted, close-mic'd). Plays the Opening Theme's 4-note motif in retrograde (B-C-D-E), ascending instead of descending. But it's slower, quieter, pitchshifted down an octave. Ends on an open fifth (no third — ambiguous, neither major nor minor). Tape hiss and vinyl crackle fade in over final 30 seconds, consuming the piano.
- **Emotional Target**: You failed, but the world continues. The institution will replace you. Your decisions still echo.
- **Reference**: William Basinski (beauty in decay).

---

## 5. Adaptive Audio System — Technical Design

### Game State Parameters for Audio

The audio engine receives the following parameters from game systems in real-time:

| Parameter | Range | Update Frequency | Drives |
|---|---|---|---|
| `BoardSatisfaction` | 0-150 | Per annual review | Music layer intensity, string tension |
| `ActiveProjectsCount` | 0-6 | Per greenlight/cancellation | Ambience density |
| `CrisisProjectsCount` | 0-6 | Per milestone review | Crisis audio layer triggers |
| `AverageStudioMorale` | 0-100 | Per quarter | Background morale ambience |
| `PortfolioPrestige` | 0-100 | Per shipped game | Music key center (minor vs. major tonality) |
| `PortfolioProfit` | 0-100 | Per shipped game | Music tempo/confidence |
| `PortfolioPopulism` | 0-100 | Per shipped game | Music brightness/accessibility |
| `CurrentPhase` | Greenlight / Milestone / E3 / Review | Per UI state | Music stem selection, ambience profile |
| `GameYear` | 1-10 | Per year transition | Era-specific tech sounds, music evolution |
| `PlayerInDecision` | bool | Per frame | Silence triggers, music duck |

### Middleware Architecture (FMOD Recommended)

**Why FMOD over Wwise**: FMOD's parameter-driven mixing and snapshot system is ideal for the gradual, continuous audio state changes in GREENLIGHT. Wwise excels at event-driven, action-heavy games. This is a contemplation-heavy game.

**FMOD Structure**:

**Master Mixer Hierarchy**:
```
Master
├── Music (VCA)
│   ├── Stems_Year1-3
│   ├── Stems_Year4-6
│   ├── Stems_Year7-8
│   ├── Stems_Year9-10
│   └── Authored_Cues
├── SFX (VCA)
│   ├── UI_Feedback
│   ├── Greenlight_Table
│   ├── Milestone_Events
│   ├── Studio_Interactions
│   └── Era_Events
├── Ambience (VCA)
│   ├── Office_RoomTone
│   ├── Boardroom_RoomTone
│   ├── Studio_Morale_Layers
│   └── Era_Tech_Ambience
└── Voiceover (VCA) [if implemented]
    ├── Board_Members
    └── Studio_Reps
```

**Snapshot System** (dynamic mix states):

| Snapshot | Active During | Mix Changes |
|---|---|---|
| `Snapshot_GreenightTable` | Pitch evaluation | Music -6 dB, Ambience -8 dB, UI_Feedback +0 dB (decision focus) |
| `Snapshot_MilestoneReview` | Project management | Music -3 dB, SFX +0 dB, Ambience -4 dB |
| `Snapshot_BoardReview` | Annual evaluation | Music +3 dB, Ambience (Boardroom) +0 dB, all other -12 dB |
| `Snapshot_Crisis` | Crisis alert active | Music (tension stems only), SFX (crisis alerts) +6 dB, Ambience lowpass 800 Hz |
| `Snapshot_Silence` | Post-cancellation, Restraint | All -18 dB except RoomTone +3 dB |
| `Snapshot_E3` | E3 event | Music (E3 Theme) +6 dB, Ambience (convention) +0 dB, all else -10 dB |

**Parameter-Driven Events**:

*Example: Music Intensity System*
```
Event: "Music_MidGame_Loop"
Parameters:
  - BoardPressure (0-100, maps to BoardSatisfaction inversely)
  - CrisisLevel (0-6, maps to CrisisProjectsCount)

FMOD Logic:
  - Stem_Cello (always playing, volume 0 dB)
  - Stem_Pad (plays if BoardPressure < 50, volume scales 0-50 range)
  - Stem_Viola_Tension (plays if BoardPressure > 60, volume scales 60-100 range)
  - Stem_Piano_Percussive (plays if CrisisLevel > 1, volume = CrisisLevel * 2 dB)
  - Stem_StringCluster (plays if BoardPressure > 85, volume = (BoardPressure - 85) * 0.5 dB)

Crossfade Time: 2 seconds per stem enable/disable
```

*Example: Portfolio Matrix Sonification*
```
Event: "PortfolioMatrix_Hum"
Parameters:
  - Prestige (0-100)
  - Profit (0-100)
  - Populism (0-100)

FMOD Logic:
  - Oscillator_E (Prestige voice): Volume = Prestige * 0.3 dB, Frequency = 165 Hz + (Prestige * 0.5 Hz)
  - Oscillator_A (Profit voice): Volume = Profit * 0.3 dB, Frequency = 220 Hz + (Profit * 0.5 Hz)
  - Oscillator_C (Populism voice): Volume = Populism * 0.3 dB, Frequency = 130 Hz + (Populism * 0.5 Hz)

Filter: Lowpass at 800 Hz (keeps sonification subliminal)
Output: Routed to Ambience VCA at -20 dB (felt, not heard)
```

### Spatial Audio (Not Applicable)

GREENLIGHT is a 2D UI-driven game. No 3D spatial audio required. All audio is stereo, center-weighted. The "space" is psychological, not physical.

### Dynamic Range Target

**LUFS Target**: -18 LUFS (integrated) for music and ambience. This is quieter than most games (typical target: -14 LUFS) but appropriate for a contemplative experience. The player should be able to hear the game at moderate volume without fatigue.

**Peak Target**: -6 dBFS (true peak). Headroom for mix safety.

**Dynamic Range**: 12-18 dB between quietest (office ambience) and loudest (E3 theme, crisis alerts) moments. Compression used sparingly — this is not a loud game.

### Audio File Specifications

| Asset Type | Format | Sample Rate | Bit Depth | Compression | Notes |
|---|---|---|---|---|---|
| Music Stems | WAV | 48 kHz | 24-bit | None (lossless) | Compressed to Vorbis OGG at build time (quality 8) |
| SFX (UI, Interactions) | WAV | 48 kHz | 24-bit | None | Compressed to Vorbis OGG (quality 6) |
| Ambience Loops | WAV | 48 kHz | 24-bit | None | Compressed to Vorbis OGG (quality 7) |
| Voiceover (if used) | WAV | 48 kHz | 24-bit | None | Compressed to Vorbis OGG (quality 8) |

**Vorbis OGG Rationale**: Smaller file size than WAV, higher quality than MP3, native Unity support. Quality 6-8 is transparent for game audio.

**Total Audio Budget Estimate**: 800 MB - 1.2 GB (uncompressed), 200-350 MB (compressed).

---

## 6. Mix Hierarchy

Audio priority tiers when multiple sounds overlap. Lower tier numbers take priority.

### Tier 1 (Critical — Always Audible, Never Masked)

- Greenlight stamp (approval / rejection)
- Cancellation notice audio
- Board evaluation dialogue / key notifications
- Crisis alert sounds

**Mix Rule**: Tier 1 sounds trigger momentary ducking of Tiers 2-4 by -8 dB for duration + 500ms tail.

### Tier 2 (High Priority — Core Feedback)

- Intervention confirmations
- Morale bar shifts
- Metacritic score reveal
- Trust change notifications
- Counter-offer responses
- Email arrival (studio correspondence)

**Mix Rule**: Tier 2 sounds duck Tiers 3-4 by -4 dB. Do not duck Tier 1.

### Tier 3 (Medium Priority — Contextual Feedback)

- UI navigation clicks
- Hover sounds
- Portfolio matrix shifts
- Pitch card interactions
- Era event notifications
- Game shipment audio
- Sales reports

**Mix Rule**: Tier 3 sounds do not duck other tiers. Can be masked by Tier 1-2.

### Tier 4 (Low Priority — Ambience & Music)

- Music (all stems)
- Room tone / office ambience
- Studio morale background layers
- Era-specific tech hum
- Background crowd/convention audio (E3)

**Mix Rule**: Tier 4 is always lowest priority. Gets ducked by Tiers 1-3. Automatically attenuates when UI interaction density is high (>3 SFX triggers per second).

### Ducking Implementation (FMOD Sidechain Compressor)

**Tier 1 Sidechain**:
- Trigger: Any Tier 1 sound plays
- Target: All other VCAs
- Ratio: 10:1 (hard duck)
- Threshold: -24 dB
- Attack: 10ms
- Release: 500ms
- Gain Reduction: Up to -8 dB

**Tier 2 Sidechain**:
- Trigger: Any Tier 2 sound plays
- Target: Tier 3 & 4 VCAs only
- Ratio: 6:1
- Threshold: -18 dB
- Attack: 20ms
- Release: 400ms
- Gain Reduction: Up to -4 dB

**Music Auto-Duck on High UI Activity**:
- Trigger: >3 UI events per second (rapid clicking, browsing)
- Target: Music VCA
- Ratio: 4:1
- Threshold: -12 dB
- Attack: 100ms
- Release: 1000ms
- Gain Reduction: Up to -6 dB
- **Result**: Music fades slightly during intense UI interaction, returns when player slows down.

---

## 7. Implementation Plan & Technical Requirements

### Phase 1: Pre-Production (Weeks 1-4)

**Deliverables**:
1. Core SFX prototypes (greenlight stamp, rejection stamp, morale shift) — 10 key sounds
2. Office ambience loops (3 variations: neutral, tense, crisis)
3. Music sketch (1 minute of Stems_Year1-3 as proof-of-concept)
4. FMOD project template with snapshot structure

**Goal**: Prove the sonic thesis. Does the stamp feel good? Does the ambience support contemplation? Can we hear the difference between a healthy and dying project through audio alone?

**Playtest Focus**: Paper prototype with core SFX. Does the stamp satisfy? Does restraint "feel" like a decision when accompanied by silence swell?

### Phase 2: Vertical Slice (Weeks 5-12)

**Deliverables**:
1. Complete SFX set for Greenlight Table (all stamp variants, pitch interactions, budget ticker)
2. Complete SFX set for Milestone Review (intervention confirmations, morale audio, crisis alerts)
3. Music Stems_Year1-3 (full 4-stem system, 3-minute loop per stem)
4. Adaptive music system in FMOD (parameter-driven stem mixing functional)
5. Studio archetype audio signatures (email notifications, pitch presentation styles)
6. Era Event notification SFX (3 variations: positive, neutral, disruptive)

**Goal**: Full audio support for a 1-year campaign slice (Year 1 playable with complete audio).

**Playtest Focus**: Does the adaptive music system feel responsive? Can players identify studio archetypes by their audio signatures? Is the mix hierarchy clear (do important sounds always cut through)?

### Phase 3: Full Production (Weeks 13-32)

**Deliverables**:
1. All SFX categories completed (3.1 through 3.8, ~200 unique sounds)
2. Music Stems_Year4-6, Year7-8, Year9-10 (12 stems total across all phases)
3. All authored music cues (Opening, E3, Board, Termination, Legacy — 5 pieces)
4. Complete ambience system (office, boardroom, studio morale layers, era tech ambience)
5. Full FMOD integration with all game state parameters
6. Portfolio Matrix sonification system
7. Era-specific audio (console launches, tech ambience shifts per year)

**Goal**: Complete audio coverage of 10-year campaign. Every interaction, every state, every moment has considered sound design.

**Playtest Focus**: Full campaign playtests with audio-focused feedback. Track fatigue (do sounds become annoying on repeat?), emotional engagement (do players cite audio moments in post-session interviews?), mix clarity (can players always hear what matters?).

### Phase 4: Polish & Mastering (Weeks 33-40)

**Deliverables**:
1. Mix balance pass (adjust all VCA levels, refine ducking, optimize dynamic range)
2. Variant expansion (add 2-3 variants to high-repeat sounds to reduce fatigue)
3. Accessibility pass (ensure critical audio has visual counterparts for hearing-impaired players)
4. Audio bug fixes and edge-case handling
5. Final mastering pass (LUFS normalization, loudness metering, platform compliance)
6. Audio asset optimization (Vorbis compression, file size reduction, streaming setup)

**Goal**: The audio is invisible. It supports, never distracts. It disappears into the experience.

**Playtest Focus**: Long-session playtests (3+ hours). Do sounds remain satisfying? Is there listener fatigue? Are there any jarring transitions or mix imbalances?

---

### Technical Requirements

**Engine**: Unity 2021.3 LTS or later (native FMOD support via integration)

**Middleware**: FMOD Studio 2.02+
- **Why FMOD**: Parameter automation, snapshot system, stem mixing, adaptive music tools. Industry-standard for non-action games.
- **License**: FMOD Indie (free for projects under $200K budget). Upgrade to FMOD Commercial if project scales.

**Audio Hardware for Development**:
- **Monitoring**: Nearfield studio monitors (KRK Rokit 5 G4 or equivalent) + reference headphones (Audio-Technica ATH-M50x or Sony MDR-7506). Critical: do not mix exclusively on headphones.
- **Recording**: Condenser microphone (Rode NT1-A or equivalent) for foley and ambience recording. USB audio interface (Focusrite Scarlett 2i2 or equivalent).
- **Foley Props**: Office furniture (desk, filing cabinet, leather chair), paper stock (various weights), rubber stamps, old office equipment (CRT monitor, mechanical keyboard, landline phone).

**Audio Plugins (DAW)**:
- **DAW**: Reaper (affordable, professional) or Logic Pro X (Mac) / Ableton Live (composition + sound design)
- **Essential Plugins**:
  - iZotope RX (audio repair, noise reduction, declicking)
  - FabFilter Pro-Q 3 (surgical EQ)
  - Valhalla VintageVerb (reverb for room ambience)
  - SoundToys Decapitator (analog saturation for warmth)
  - Native Instruments Kontakt (orchestral sample libraries)

**Sample Libraries** (for music composition):
- **Strings**: Spitfire Audio LABS (free, excellent starter) or Cinematic Studio Strings (premium)
- **Piano**: Native Instruments The Gentleman (felt piano, perfect for this score) or Spitfire Soft Piano
- **Analog Synths**: Arturia V Collection (Moog, Juno, Prophet emulations) or U-He Diva
- **Ambience**: Built from field recordings + FreeSound.org (CC0 licensed office ambience)

**File Management**:
- **Asset Naming Convention**: `Category_Action_Variant_##.wav`
  - Example: `SFX_Stamp_Greenlight_01.wav`, `Music_Stem_Year1_Cello.wav`
- **Folder Structure**:
```
/Audio
  /SFX
    /GreenightTable
    /MilestoneReview
    /StudioInteractions
    /EraEvents
    /Portfolio
    /UI
    /Endgame
  /Music
    /Stems
      /Year1-3
      /Year4-6
      /Year7-8
      /Year9-10
    /Authored
  /Ambience
    /Office
    /Boardroom
    /StudioMorale
    /EraTech
  /FMOD_Project
```

**Version Control**: All audio source files (.wav, .aif) stored in LFS (Large File Storage) if using Git. FMOD project files (.fspro, .fsb) tracked in standard Git.

---

### Audio Budget (Time Estimate)

**Assumptions**: Single audio designer/composer (ECHO) working full-time.

| Phase | Duration | Hours/Week | Total Hours | Key Deliverables |
|---|---|---|---|---|
| Pre-Production | 4 weeks | 40 | 160 | Core SFX prototypes, ambience, music sketch |
| Vertical Slice | 8 weeks | 40 | 320 | Full Year 1 audio, adaptive system |
| Full Production | 20 weeks | 40 | 800 | Complete audio for 10-year campaign |
| Polish & Mastering | 8 weeks | 40 | 320 | Mix, variants, optimization, mastering |
| **TOTAL** | **40 weeks** | **40** | **1600** | **Comprehensive audio** |

**Additional Contractor Needs**:
- **Foley Recording Session**: 2 days (8 hours) with professional foley artist for physical stamp mechanisms, office furniture. Cost: $1,200-2,000.
- **Orchestral Recording** (if budget allows): Legacy Montage theme recorded with live 8-piece chamber ensemble (strings + brass). 4-hour session. Cost: $3,000-5,000. Alternative: Use sample libraries (Spitfire, Cinematic Studio) for 95% of the quality at 5% of the cost.
- **Voice Acting** (if implemented): Board members + studio reps. 4-6 actors, 2-hour sessions each. Cost: $2,400-4,000 total. Optional: Game works with text-only UI + abstracted vocal textures.

---

## 8. Silence Map

Silence is not absence. Silence is a compositional choice. Here's where GREENLIGHT deliberately removes sound to create impact.

### Critical Silence Moments

**1. Post-Cancellation Silence (8 seconds)**
- **When**: Immediately after player cancels a project.
- **What's Silent**: All music, all ambience layers, all UI sounds.
- **What Remains**: Room tone only (barely audible office hum at -24 dB).
- **Why**: The player just killed a game. Let them sit with that. No score tells them how to feel. The silence is the consequence.
- **Implementation**: Triggered by `Event_ProjectCancelled`. FMOD snapshot `Snapshot_Silence` activates for 8 seconds, then crossfades back to `Snapshot_MilestoneReview` over 3 seconds.

**2. Restraint Decision Silence (while choosing)**
- **When**: Player hovers over "No Intervention" option in milestone review.
- **What's Silent**: Music fades to -12 dB (still present but distant). All SFX stop.
- **What Remains**: Room tone + very subtle breath (player character exhale, 1 breath per 6 seconds).
- **Why**: Choosing not to act must feel like a deliberate, powerful choice. Silence gives it gravity.
- **Implementation**: Hover state triggers gradual music duck (1-second fade). If player selects Restraint, music stays ducked until next quarterly event.

**3. Opening Scenario — First 15 Seconds**
- **When**: Game opens. Inherited cancellation scenario loads.
- **What's Silent**: Everything except room tone.
- **What Remains**: Office ambience at -18 dB. A distant fluorescent light hum.
- **Why**: The player needs to understand this is not a cheerful game. Start in silence. Let the first sound they hear be the Opening Theme cello.
- **Implementation**: Game loads with `Snapshot_Silence` active. After 15 seconds, Opening Theme fades in over 8 seconds.

**4. Pre-Board Evaluation — The Walk to the Boardroom (5 seconds)**
- **When**: Transition from quarterly results to annual board evaluation.
- **What's Silent**: All music stops. All UI sounds stop. All project ambience stops.
- **What Remains**: Footsteps (player character walking down hallway, distant, reverberant). A door opening. Then boardroom ambience fades in.
- **Why**: This is a threshold moment. The player is entering judgment. Mark it with silence and footsteps.
- **Implementation**: Cutscene audio (non-interactive 5-second sequence). Footsteps recorded in actual hallway with marble floors for authenticity.

**5. Termination Scene — After Gavel (3 seconds)**
- **When**: After board terminates player. Gavel strikes, papers swept away.
- **What's Silent**: Absolute silence. Not even room tone.
- **What Remains**: Nothing.
- **Why**: This is the void. You failed. The institution is done with you. Silence is the most brutal feedback.
- **Implementation**: Hard cut to -inf dB for 3 seconds. Then office door closing sound triggers epilogue sequence.

**6. Between Fiscal Years — Midnight on December 31 (2 seconds)**
- **When**: Calendar rolls over to new year.
- **What's Silent**: All music, all active project sounds, all UI.
- **What Remains**: Clock ticking (10 ticks), then single bell tone (midnight chime, distant church bell, C4 fundamental).
- **Why**: Year transitions are thresholds. Silence acknowledges the passage of time, the weight of a year's decisions.
- **Implementation**: Era transition animation triggers `Snapshot_Silence` for 2 seconds, then new year ambience fades in.

### Negative Space in Mix (Permanent Subliminal Silence)

**Frequency Gaps**:
- **40-80 Hz**: Reserved exclusively for crisis alerts, termination low-frequency drone, and legacy montage sub-bass. Kept empty otherwise. When sound appears here, player's body reacts before their mind does.
- **8-12 kHz**: Reserved for critical UI feedback (greenlight stamp transients, morale bar chimes). Ambience and music stay below 8 kHz. This keeps high-frequency space "clean" so urgent sounds always cut through.

**Temporal Gaps**:
- After every major decision (greenlight, intervention, cancellation), a 200-400ms gap before next sound event. Lets the decision "land."
- Music does not play continuously. Target: 60% of gameplay has music, 40% has ambience-only. Music returns after completing actions, fades during contemplation. Player should notice when music starts more than when it stops.

---

## 9. Accessibility Considerations

Audio is critical to GREENLIGHT's feel, but the game must be fully playable for hearing-impaired players.

### Visual Redundancy for Critical Audio

Every Tier 1 (critical) audio event has a visual equivalent:

| Audio Event | Visual Equivalent |
|---|---|
| Greenlight stamp | Green checkmark animation + project card highlight |
| Rejection stamp | Red X animation + pitch card dimming |
| Crisis alert | Red border pulse on project card + exclamation icon |
| Cancellation notice | Project card collapses with red overlay + "CANCELLED" text |
| Morale change | Bar animation + numerical value change |
| Trust change | Relationship UI icon shift + color change |
| Board warning | Text notification + board member portrait animation |
| Era event | Full-screen notification with text + icon |

### Audio Subtitles / Captions

If voiceover is implemented (board members, studio reps):
- Full closed captions with speaker identification.
- Sound effect captions for critical audio: "[Stamp thuds decisively]", "[Project cancelled - papers swept away]", "[Morale drops — glass draining]".
- Adaptive music state changes can be captioned abstractly: "[Music intensifies — pressure mounting]", "[Silence — contemplation]".

### Player Audio Options

**Settings Menu — Audio Panel**:
- Master Volume (0-100%)
- Music Volume (0-100%)
- SFX Volume (0-100%)
- Ambience Volume (0-100%)
- Voiceover Volume (0-100%, if implemented)
- **Dyslexia-Friendly Audio Mode**: Extends all audio feedback durations by 50% (gives more time to process both audio and visual information simultaneously).
- **Reduce Audio Complexity**: Disables subliminal ambience layers (studio morale background, portfolio sonification), keeps only core SFX and music. For players with auditory processing sensitivities.
- **Visual-Only Mode**: Mutes all non-critical audio, keeps only UI feedback sounds at -6 dB. For players who prefer visual-primary experience.

---

## 10. Audio Narrative Integration

Audio is not decoration. Audio IS narrative in GREENLIGHT. Every sound tells the player something about the world, the stakes, or themselves.

### Studio Archetype Audio Signatures

Players should be able to identify studio archetypes by sound alone after 2-3 hours.

**Auteur Studios**:
- Pitch presentations: Hand-recorded, slightly lo-fi (passionate, unpolished). Occasional mic feedback (emotion overflowing the format).
- Email notifications: Slightly detuned bell (+3% pitch, "precious").
- Project ambience: Sparse but intense. Single sustained cello note in background at -20 dB (artistic focus).
- Morale shifts: Dramatic. Rising morale = orchestral swell. Falling morale = string dissonance.

**Reliable Studios**:
- Pitch presentations: Clean corporate templates, smooth slide transitions, no technical artifacts (professional, sterile).
- Email notifications: On-pitch bell, neutral.
- Project ambience: Steady typing, coffee machine, neutral office hum (productive, stable).
- Morale shifts: Moderate. No extreme swings. Functional, not emotional.

**Volatile Studios**:
- Pitch presentations: Erratic pacing, slides load slowly, occasional glitch sounds (chaos energy, unpolished brilliance).
- Email notifications: Random pitch variance ±8% (unstable, unpredictable).
- Project ambience: Variable. High morale = energetic typing + distant laughter. Low morale = complete silence (team has imploded).
- Morale shifts: Extreme. Wild swings in volume and tone. Exhausting but exciting.

**Fading Studios**:
- Pitch presentations: Scanned images of old press kits, scan line artifacts, tape hiss (living in the past, nostalgia as armor).
- Email notifications: Slightly muffled (-5% pitch, like an old recording).
- Project ambience: Slow typing, long pauses, distant phone ringing unanswered (the glory days are over).
- Morale shifts: Sad. Falling morale = single piano note descending (resignation). Rising morale = tentative hope (fragile).

### Audio as Foreshadowing

**Subtle Audio Warnings** (player may not consciously notice, but will "feel"):
- **Studio About to Leave**: Email notifications from that studio become slightly distorted (pitch wobble, 0.5 Hz LFO) starting 2 quarters before departure.
- **Project About to Fail**: Project card ambience gets "thinner" (highpass filter at 800 Hz, removes warmth) when Quality_Seed drops below 50.
- **Board Tension Rising**: Office ambience gains a barely audible 2 Hz pulse (heartbeat frequency) when BoardSatisfaction < 80. Intensifies as it drops.
- **Era Event Coming**: Music begins incorporating sounds from the upcoming era (e.g., 1 quarter before Wii launch, toy-like synth sounds bleed into the music at -24 dB — subliminal foreshadowing).

### Audio as Reward

**Rare, Earned Audio Moments**:
- **Magnum Opus Offer**: When Auteur studio offers their once-in-a-career pitch, the email notification is replaced by a full 5-second orchestral swell (triumphant, prestigious). First time player hears "big" music outside of E3 or endgame.
- **Decade Milestone**: Completing Year 10, reaching the Legacy Montage — this is the only time the full orchestral score plays at foreground volume. Earned, not given.
- **Perfect Portfolio**: If player achieves all three axes above 70 simultaneously, the Portfolio Matrix sonification resolves into a perfect C Major triad (no dissonance, pure harmony, 3 seconds). Happens maybe once per playthrough. Players will chase this sound.

---

## Appendix: Production Notes for ECHO

*These are notes to myself. Principles I'm holding through production.*

**1. The stamp is the heartbeat of this game.**
If I get one sound perfect, it's that greenlight stamp. I'll record 50 takes. I'll try every rubber compound, every desk surface, every mic position. That stamp has to make players want to greenlight things just to hear it again. Reference: The typewriter in *Resident Evil*. The sword unsheathe in *Ghost of Tsushima*. Sounds so good they become reasons to act.

**2. Silence is not laziness.**
When I cut the music after a cancellation, that's not "saving on asset production." That's the most important audio decision in the scene. Defend silence aggressively. If a producer says "it feels empty," push back. Empty is the point.

**3. Subliminal layers are 80% of the emotion.**
The player will never consciously hear the studio morale ambience or the portfolio sonification. But they'll *feel* it. If I do my job right, they won't know why a high-morale project feels "lighter" than a low-morale one — they'll just feel it. That's the craft. That's why we do this work.

**4. The era is in the hum.**
CRT monitors hum at a different frequency than LCDs. Dial-up modems sound like the 2000s rendered as audio. A Windows XP startup chime is a time machine. Use the tech sounds not as nostalgia bait, but as environmental truth. The player exists in 2005. The audio should prove it.

**5. Music is a support act.**
I'm a composer. I love melody. I love climax. I have to kill both instincts here. GREENLIGHT's music is ambient architecture, not performance. If the player remembers a melody from this game, I've failed. If they remember *feeling something* they can't name, I've succeeded. Reference: *Severance* score. Barely there. Impossible to forget.

**6. Audio IS game feel.**
The designer thinks the greenlight decision is the core loop. I know the truth: the stamp sound is the core loop. The cello drone during a crisis is the tension system. The portfolio sonification is the strategy layer. Audio isn't polish. Audio is the invisible half of the game. If I cut every sound and the game still "works," we've both failed.

**7. Players will spend 15 hours with these sounds.**
Repetition is the enemy. Every high-frequency sound needs 3+ variants. Every music loop needs to be 2+ minutes to avoid short-loop madness. Every ambient layer needs randomized micro-variations. Fatigue is silent death. Fight it with variance, always.

**8. The humans are in the breath.**
When a studio representative is nervous, they breathe differently. When a board member is angry, they shift in their chair. These sounds are -20 dB, maybe -24. Players won't "hear" them. But their body will recognize them. We're wired to hear breath, footsteps, tension in another human. Use that wiring. Be subtle. Be true.

**9. Test with your eyes closed.**
Once per week, sit with the game and close your eyes. Play through a session by audio alone. Can you tell when morale drops? Can you feel the difference between a healthy project and a dying one? If you can't hear it blind, it's not there yet. Audio is not paint on top of design. It's structure underneath.

**10. This game is about power that isolates.**
The publisher sits in a chair and signs papers that change lives they'll never see. The distance between the decision and the human cost is the wound. Audio must communicate that distance. Corporate sounds (stamps, printers, boardrooms) are close, intimate, tactile. Human sounds (breaths, typing, morale ambience) are distant, muffled, ghostly. The player is surrounded by bureaucracy and haunted by people. That's the mix. That's the thesis.

---

**End of Document**

*Close your eyes. Listen to the space between the stamp and the consequence. That's where the game lives.*

-- ECHO
