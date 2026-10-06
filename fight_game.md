# Realistic Boxing Simulator and Interactive Academy
## Professional implementation research and development plan
Research date: 6 October 2026. Planning currency: USD.

## 1. Executive decision
Confirmed product vision, 6 October 2026: create a playable and watchable boxing experience that looks and feels like a live professional fight. Authentic fighter behavior, convincing contact and movement, televised presentation, arena atmosphere and event entertainment are core requirements. Research, Academy, progression and community features support this experience. The following Section 23 defines the presentation scope and first integrated acceptance milestone.

The concept is technically feasible: select two fighters, control one or watch an AI bout, and inspect techniques and exchanges in slow motion. The recommended starting product is an offline Windows PC simulator with two original fighters, one ring, controller support, a short bout, and a small coach-reviewed training library. The complete product includes Offline computer-opponent play and Online human-versus-human coin matches. Stage Online delivery after the offline combat foundation is proven; expand licensed celebrities, additional platforms and camera assessment separately.

This report distinguishes documented technology capabilities from proposed design choices. Architecture, staffing, budgets, schedules, acceptance targets, and commercial positioning below are planning recommendations, not vendor quotations or measured results. There has been no implementation, hardware benchmark, licensing negotiation, customer survey, or coaching trial for this project.

The central challenge is not generating a realistic face. It is making movement, punch contact, defense, stamina, tactical decisions, and replay agree with each other. A fighter who looks convincing but slides across the floor or lands visibly missed punches will undermine both gameplay and teaching.

## 2. Product definition and competitive position
Use two main sections, Offline and Online, sharing the same combat foundation. Academy and Watch are supporting experiences within Offline. The following experience table describes their functions:

| Experience | User workflow | Purpose |
|---|---|---|
| Fight | Select fighters, rules and difficulty; control a fighter | Enjoyment and tactical practice |
| Watch | Select two AI fighters, camera and speed | Explore contrasting styles |
| Academy | Choose technique, view demonstration, inspect stages, try drill | Learn boxing concepts and recognize mistakes |

Undisputed already markets licensed boxers, varied punches, footwork, defense, fighter traits and multiple AI styles [S1]. These features alone are not a defensible differentiation. The proposed differentiator is an integrated learning loop: fight → inspect a documented exchange → see a coach-approved correction → practice it → revisit the same situation. This is a positioning hypothesis requiring customer validation.

Interview 15–25 potential users across casual players, boxing fans, beginners and coaches. Ask what they currently use, which frustration matters, what device they own, and whether they would use the prototype again. Test realistic graphics versus clearer instructional presentation. Separately evaluate willingness to pay; enthusiasm is not a purchase commitment.

Do not market simulated Tyson-versus-Ali outcomes as scientifically accurate historical predictions. Style representation can be plausible without identifying an objectively correct winner.

## 3. Scope and platform strategy
Platform decision, 6 October 2026: the user confirmed a native Mobile game plus Web companion. See [platform_structure.md](platform_structure.md) for the shared product structure and feature ownership. The earlier Windows/controller-first recommendation below is retained as a development prototype option, not a confirmed public launch platform. Playable browser combat is outside initial scope; Android/iOS launch order remains open. Engine choice must follow native mobile device benchmarks.

Proposed first playable: two fictional heavyweight archetypes, a pressure fighter and an out-boxer; one weight class and orthodox stance; jab, cross, lead hook and rear uppercut; head/body targeting where supported; guard, slip, basic step and pivot; short rounds; stamina; basic knockdown and reset; AI difficulty; replay and five introductory lessons.

Southpaw, stance switching, clinches, rope entanglement, detailed cuts, full career, licensed roster, online ranked fights and camera coaching are expansions. They introduce substantial additional animation and rule combinations. A feature may be needed eventually without belonging in the first playable.

Target Windows/controller first. Propose a 60 FPS performance tier with explicit minimum and recommended hardware established by profiling. Do not promise cinematic settings on integrated graphics. Console work requires platform access, certification and porting. Mobile needs a separate rendering and control budget. VR needs another interaction and comfort design; it is not simply the PC game with a headset.

## 4. Engine and technical tools
Recommended default: Unreal Engine, C++ for authoritative combat/state and Blueprints for content assembly and UI. This is an engineering judgment for the visual ambitions, not proof that Unreal outperforms Unity for this exact project.

Epic documents Motion Matching, physics/animation blending, scalable MetaHuman assets and replay support [S2–S5]. Unity remains a credible alternative when the team already has strong Unity expertise or mobile is the primary target [S6]. Team competence and a measured prototype matter more than an engine feature list.

Use Blender or equivalent for modeling/rigging, professional texture tooling if budget allows, Git LFS or Perforce for large binary assets, an issue tracker, automated builds, crash reporting and frame profiling. Pin the engine and plugin versions after evaluation; current documentation may describe a different version from the production branch. Review every sample, marketplace asset and plugin license separately.

## 5. Runtime architecture
Separate six responsibilities:

1. Input produces action requests, including timestamp and desired direction/target.
2. Combat simulation validates requests, advances action phases and resolves contact.
3. Presentation uses the resolved state for animation, sound, camera and effects.
4. AI observes permitted information and requests actions through the same combat API.
5. Replay stores enough state and events to reconstruct the exchange.
6. Training interprets those events using reviewed teaching rules.

The simulation owns action legality, contact results, stamina, damage and outcomes. A visual montage or camera effect must not independently decide a hit. UI and learning feedback consume the same authoritative exchange record.

Use a fixed combat step; evaluate 60 Hz initially and higher collision sampling/substeps if fast movement needs it. These are separate from rendering FPS. Document input buffering, cancellation windows, simultaneous hits, action priority and time scaling. Offline repeatability should be tested on the supported build; do not assume a general physics engine is cross-platform deterministic.

Core data contracts:

| Record | Minimum information |
|---|---|
| FighterProfile | Version, body measurements, stance, capabilities, style parameters, animation bindings, provenance |
| TechniqueDefinition | ID/version, allowed stance, phases, movement constraints, target, collision windows, stamina costs, follow-ups |
| CombatSnapshot | Tick/time, positions, guard, action and phase, resources, state flags |
| CombatEvent | Tick, attacker/defender, technique, contact geometry, block outcome, consequence and reason codes |
| LessonDefinition | Objectives, reviewed cues, demonstration, drill conditions, success criteria and reviewer |
| ReplayManifest | Build/schema versions, fighter/technique versions, snapshots/events, seeds and content compatibility |

Content tools should validate missing animations, invalid transitions, out-of-range parameters and incompatible replay versions before packaging.

## 6. Fighter DNA and Technique DNA
Fighter DNA should contain interpretable parameters: reach, stance, movement preferences, preferred range, combination library, guard style, pressure tendency, counter preference, risk appetite, reaction delay, stamina expenditure and recovery. Separate physical capacity from tactical preference. A high aggression value should influence decisions, not silently increase punch power.

Technique DNA describes a movement and its legal use. A jab has preparation, acceleration, extension/contact, retraction and guard recovery. Define which phases permit movement, whether the opposite hand remains defensive, which contact windows are active and which follow-ups are allowed. Use seconds/ticks and versioned curves rather than undocumented animation frame assumptions.

Neither DNA requires machine learning in v1. These are useful authoring abstractions, not established scientific measurements. For famous fighters, record era, weight class, footage sources, annotation uncertainty and coach assessment. One athlete's career should not be collapsed into an unqualified timeless profile.

## 7. Character and motion production
Produce original concept sheets and a shared skeleton first. Build body shape, face, gloves, trunks and boots; rig shoulders, spine, hips, feet and wrists; test deformation under punch extremes. Scan authorized performers if feasible. MetaHuman can accelerate human appearance work, but boxing bodies, gloves and contact deformation still need specialist work. Epic provides scalability guidance; benchmark close-up and gameplay assets separately [S4].

Capture qualified boxers performing controlled demonstrations. Obtain performer agreements and asset rights. Record normal-speed, slow coached demonstrations, strikes, misses, recovery, defensive movement, transitions, moving attacks and controlled reaction references. Avoid dangerous impact capture for visual authenticity.

Pipeline: capture → solve → clean → retarget → coach review → annotate action/contact/recovery → integrate → gameplay test → optimize. Mocap is an input, not an automatically finished gameplay asset. Foot sliding, occlusion, glove position and transitions typically require cleanup.

Use constrained authored attacks and a locomotion system initially. Motion Matching can improve pose selection from an appropriate database [S2], but cannot supply boxing skills absent from the data. Add limited IK/warping for range adjustments, with caps to prevent unnatural limb extension. Blend localized reactions with animation and physics [S3]; reserve ragdoll for appropriate knockdown states.

Create an animation coverage matrix by stance, direction, technique, target, range and transition. Use it to estimate production volume before committing to a large roster.

## 8. Contact, defense and fight consequences
Track glove paths with swept collision volumes between samples. Define hurt volumes for legal head and body regions, separate guard volumes and rules for clean, partial, blocked, missed and invalid contacts. De-duplicate a punch against repeated overlap callbacks. Specify simultaneous exchanges, illegal targets, contact during recovery and knockdown immunity states.

Prototype with visible debug volumes and a simple opponent. Compare the resolved contact with the displayed glove position frame by frame. Huge forgiving hitboxes can make a game approachable but teach misleading distance; assistance levels should be explicit.

Use a tunable consequence model combining technique, committed effort, contact quality, defense, fatigue and balance. These are game variables, not calibrated injury probabilities. Maintain distinct short-term stamina, accumulated fatigue and recovery/stun states. Model head and body consequences separately if they improve play. Avoid arbitrary depletion that makes every bout feel identical.

Define rule profiles rather than mixing amateur and professional rules. Store rounds, breaks, permitted targets, knockdown/count policy, fouls, stoppages and scoring. World Boxing provides competition rules for its own context [S7]; a professional mode needs the intended jurisdiction/body's rules. A simplified game profile should be labeled as such. Do not treat punch count alone as an authoritative real-world scoring system.

## 9. Adaptive opponents
Begin with hierarchical states plus utility-based action selection: approach, maintain range, attack, defend, counter, recover and reposition. Epic's StateTree is one implementation option [S8].

AI observes distance, angle, visible guard/movement, elapsed cues, its own resources and a bounded history. It must not read future player inputs or hidden perfect information. Difficulty should vary reaction delay, observation uncertainty, decision quality and tactical depth rather than invisible cheating.

Track recent patterns: head/body frequency, repeated openings, retreat direction and reactions to feints. Update an opponent model with decay and minimum sample requirements. Bias actions toward learned tendencies without creating certainty after one example. Log the observation and reason for each tactical choice so adaptation can be tested.

Example: repeated body targeting raises the perceived body-threat frequency; the defender shifts protection; the attacker may choose an upstairs feint combination within its available library. Demonstrate this with controlled bot scripts, then test unpredictability and exploitability against players.

Reward-trained low-level movement is a later research track. AMP demonstrates learning stylistic physical movement from motion data [S9], not a ready-made commercial boxing system. Budget for training stability, simulator differences, inference cost, contact behavior, debugging and fallback animations. Large language models can help draft reviewed lesson text; they should not arbitrate per-frame combat or invent diagnoses.

## 10. Replay and slow motion
Epic's replay system supports recording, pausing, seeking and changing playback speed; a memory streamer is available for instant replay [S5]. Confirm whether it preserves all project-specific motion/contact data at the required precision.

Store input/actions, authoritative combat events, pose/state checkpoints and necessary animation/physics information. Inputs plus a seed alone may not reproduce a nondeterministic physics exchange. Start with a rolling 10–15-second buffer and permanent highlights; measure storage cost.

Offer 1×, 0.5×, 0.25×, pause, frame stepping, orbit camera, normal view and optional overlays. Recorded contact outcomes must remain unchanged at different playback speeds. Offline live slow motion is a separate feature; do not globally slow a competitive online match for one player.

For an explanation such as 'guard was open at contact', show the actual event and relevant guard state. If discussing why a punch landed, use explicit evidence and uncertainty. An alternative such as recovering guard is a teaching suggestion, not proof that every counterfactual would prevent the hit. Provide a restart-from-situation drill rather than inventing a guaranteed alternate outcome.

## 11. Academy and learning validation
Start with stance/guard, jab, cross, safe recovery, basic distance and defensive recognition. Expand into combinations, pivots, slips, rolls, feints, counters and ring control. Separate game-control proficiency, boxing knowledge and physical execution.

Each lesson needs a coach-reviewed objective, multi-angle demonstration, short cues, common errors, prerequisite, guided task and assessment. Permit style variations; do not present one wrist/hip/foot angle as universally correct across fighters and contexts. Beginners should see one or two cues at a time, with deeper overlays optional.

A game can teach recognition and tactics; transfer to physical skill remains unproven until evaluated. Use qualified coaches for physical instruction, avoid encouraging unsupervised sparring, and describe the product as supplementary learning. Do not equate a game score with readiness to fight.

Evaluate a pilot with pre/post knowledge tests and delayed retention. For physical transfer, use blinded coach ratings and a suitable comparison group; establish ethical/safety procedures before recruitment. Predefine the primary outcome and sample-size rationale. A small usability test supports usability claims, not broad effectiveness claims. England Boxing's coaching resources are useful references for curriculum development [S10]; check reproduction rights before embedding their material.

## 12. Camera coaching: separate feasibility project
MediaPipe Pose Landmarker estimates 33 body landmarks [S11]. That does not directly measure glove contact, impact force, subtle wrist alignment or reliable depth under all conditions. Camera feedback should initially cover only observable gross positions and movement with confidence checks.

Set calibration, framing, lighting and visibility requirements. Test motion blur, occlusion, gloves, clothing, mirrored views, camera angle and diverse body types. Report 'cannot assess' when evidence is weak. Validate each feedback label against independent coach annotations, including false corrective feedback and confidence calibration. Never infer safe punch force or injury risk from ordinary monocular video.

Prefer on-device processing; specify retention, explicit upload consent, deletion and account protections if clips are stored. Obtain jurisdiction-specific privacy review, particularly for minors and sensitive inferences. Make camera mode optional. Video upload analysis, real-time coaching and motion capture are different products with different accuracy requirements.

## 13. Celebrity and asset rights
For recognizable real fighters, obtain specialist legal review and appropriate contracts before commercial use. California's publicity framework addresses commercial uses of name, voice and likeness without consent; scope, exceptions and territorial rules require case-specific analysis [S12]. Estates can introduce additional rights questions. A renamed but recognizably copied fighter is not automatically cleared.

Create a rights register covering names, faces, voices, tattoos, photographs, broadcast clips, motion data, music, commentators, brands, belts and arena marks. Footage rights and an athlete's likeness rights are separate. Public online availability does not establish a license to reuse footage or train commercial models.

Negotiate territory, platforms, term, likeness/voice/digital-replica uses, promotional uses, approval process, advances/royalties, expiration and post-expiration handling. An athlete agreement does not necessarily clear every tattoo, sponsor or broadcast asset. Celebrity fees are unknown until quoted and are excluded from the budgets below.

## 14. Team, cost and schedule
Lean specialist team: technical/gameplay lead, animation technical artist, character artist, designer/producer, fractional boxing coach, and QA support. Add audio, UI, mocap and legal contractors as needed. The producer owns scope and acceptance; the coach approves lessons; engineering owns authoritative combat; animation owns movement coverage.

Illustrative scenarios—not market prices or commitments:

| Scenario | Staffing assumption | Duration | Labor arithmetic | Approximate planning envelope |
|---|---|---|---|---|
| Technical proof | 2 average full-time equivalents at $4k–$8k/month | 3–4 months | $24k–$64k | $35k–$100k |
| Polished vertical slice | 4–6 average FTE at $5k–$9k/month | 6–9 months | $120k–$486k | $180k–$700k |
| Focused commercial PC release | 8–12 average FTE at $6k–$10k/month | 18–24 months | $864k–$2.88m | $1.2m–$4m |

Envelopes add variable contractor/capture/tool/hardware costs and contingency. They exclude celebrity rights, major marketing, platform/store revenue shares, taxes and financing. Scenarios describe different scope, not additive guaranteed phases. Replace them with local loaded employment rates and supplier quotes before funding. A solo proof can cost less cash but take considerably longer and cannot reasonably match a large studio's content volume.

Unreal's standard game licensing lists 5% royalties above the first $1m lifetime product gross revenue, with exclusions and possible reduced-rate qualifications governed by its EULA [S13]. Unity lists free Personal eligibility up to $200k revenue/funding and Pro annual pricing of $2,310 per seat in 2026 [S6]. Recheck licensing at commitment. Store onboarding is a small cost compared with development: Steam lists a $100 per-product fee with stated recoupment conditions [S14].

## 15. Professional phase gates
Phase A, weeks 1–2: user interviews, coach consultation, target hardware, controls, scope, rights policy and content matrix. Deliver product brief and measurable acceptance plan.

Phase B, weeks 3–6: gray-box ring, two proxy fighters, jab/cross, contact/guard, movement, input timing and debug replay. Deliver a repeatable contact harness. Stop expanding scope if hits do not match visuals.

Phase C, weeks 7–12: production-quality representative motion, stamina, basic AI and five reviewed lessons. Deliver a technical proof, not a full commercial game.

Phase D, subsequent 3–6 months if gates pass: visual slice, two distinct fighters, UI, audio, knockdowns, expanded replay and coaching review. Conduct external playtests and update budget.

Phase E: commercial production only after evidence supports retention, differentiation, affordable content throughput and stable performance. Plan online/console work as separately funded milestones.

Suggested acceptance targets, to be finalized before testing:

- Contact: adjudicated cases across misses, guard, head/body and simultaneous attacks; no repeated hit per action; visible/resolved disagreement documented and reduced below the chosen tolerance.
- Replay: every hit/miss and bout result preserved across seeking and speed changes in the regression corpus.
- Performance: define hardware/settings; target 60 FPS with representative crowd, effects and both fighters; inspect frame-time percentiles, not averages alone.
- AI fairness: zero future-input access by design review; reaction bounds verified; adaptation tested against a nonadaptive baseline and scripted patterns.
- Instruction: every shipped lesson signed off by a qualified coach; feedback ties to recorded evidence; uncertain assessments abstain.
- Usability: first-time users complete fighter selection, basic bout and slow replay without assistance; record completion/error rates and revise confusing steps.

Automate combat regression and replay compatibility tests. Use manual animation/contact review, accessibility checks, long AI bouts, controller disconnects and interrupted save/replay tests. Keep separate defect categories for simulation errors, visual errors and teaching errors.

## 16. Online and business expansion
Online boxing is sensitive to latency because fast contacts and defensive windows need consistent adjudication. Epic explicitly documents networked-physics latency challenges [S15]. Evaluate server authority, prediction, reconciliation and lag compensation against the actual combat design. Rollback is not a drop-in remedy for uncontrolled physics. Measure latency, jitter, loss and conflicting-hit cases before selling ranked competition.

A first-release business hypothesis is a premium offline game with the introductory academy included. Later options include licensed roster packs, expanded coach-reviewed lessons and gym demonstrations. Validate willingness to pay and support workload first. Avoid selling physical-training effectiveness before evidence exists; licensing advances and server operations can change unit economics substantially.

Maintain an independent boxing repository and roadmap. Any future connection with video-analysis tooling should be a defined interface and separately approved scope, rather than assuming footage analysis can automatically create usable fighter behavior.

## 17. First concrete implementation package
Before hiring a large team, prepare: product brief; combat design document; data contracts; animation/capture shot list; rights register; coach review checklist; replay schema; target-hardware profile; milestone budget; and phase-gate test plan. The first engineering experiment should be one visible jab hitting, missing and being blocked consistently, then replaying those outcomes at quarter speed. That small experiment tests the technical foundation of the larger idea.

## 18. User-defined Offline and Online modes — 6 October 2026 update
This section supersedes the earlier generic mode and online-expansion descriptions. These are product requirements, not implemented features. Offline human-versus-human matches are excluded. Online opponents are people; never silently substitute a bot when matchmaking is empty.

### Offline: play against the computer
The player selects a boxer, computer-controlled opponent and one of four difficulties:

| Difficulty | Proposed AI behavior |
|---|---|
| Beginner | Slower reactions, simple attacks, generous openings and optional cues |
| Intermediate | Basic combinations, better guard and occasional counters |
| Difficult | Better range control, feints, counters and pattern adaptation |
| Expert | Strong tactical adaptation and stamina management, with bounded human-like reactions |

Difficulty behaviors are recommendations requiring tuning. All levels obey the same contact rules. Offline has no human opponent requirement or online match entry fee. Retain training, slow-motion replay and AI exhibition/watch support. Specify whether pause/live slow motion is allowed in reward-earning bouts. Offline-earned coins are requested as a possible progression source, but earning rates and eligibility are unresolved.

### Online: two people compete with accumulated coins
The player chooses a tier and either finds an available person or invites a friend. Show overall connected players, players available in the selected tier/region, and estimated waiting time when enough data exists. Distinguish connected users from users actually available to fight. Show zero honestly; do not fabricate population or label bots as human opponents.

| Tier | Entry per player | Two-player pot | Winner payout | Winner net gain | Pot less payout |
|---|---:|---:|---:|---:|---:|
| Beginner | 250 | 500 | 475 | 225 | 25 |
| Intermediate | 1,000 | 2,000 | 1,900 | 900 | 100 |
| High Level | 5,000 | 10,000 | 9,500 | 4,500 | 500 |
| Difficult | 25,000 | 50,000 | 47,500 | 22,500 | 2,500 |
| Higher Difficult | 100,000 | 200,000 | 190,000 | 90,000 | 10,000 |
| Expert | 500,000 | 1,000,000 | 950,000 | 450,000 | 50,000 |

The user's reward values are provisionally interpreted as the total payout credited to the winner, including recovery of their own entry. The loser loses their entry. All tiers retain/remove 5% of the combined pot. This is virtual-coin accounting, not established monetary revenue. Whether retained coins go to a system account or are destroyed remains a design decision. Online tiers are stake levels, not a guarantee of player skill; maintain a separate skill rating and match within compatible skill, latency and region constraints.

Before confirmation show entry, total winner payout, winner net gain and loss on defeat. Invite flow: select friend/invite link → choose tier → recipient accepts → both confirm exact terms → reserve entries → establish match → start. An invitation alone must not debit either player. Matchmaking must never raise the stake without new consent. If no compatible opponent exists, offer waiting, another tier or Offline.

### Proposed authoritative wallet and settlement design
Use server-owned balances and an append-only transaction ledger. Match lifecycle: created → accepted → both stakes reserved → active → result verified → settled, or cancelled/refunded. Make every reservation/refund/settlement idempotent and atomic; enforce one settlement per match and prevent simultaneous matches spending the same balance. Clients send inputs, not trusted winners or balance updates.

Proposed defaults requiring product approval: refund both entries on draw or infrastructure failure; reconnect within a disclosed grace period; use server evidence for abandonment/forfeit; refund cancelled matches before start. Do not let a client disconnect automatically award itself a win or force refunds after an established defeat. Store match results, replay evidence and economy configuration versions for dispute review.

For friend matches, address collusion, repeated intentional losses, alternate accounts, bots and coin transfers disguised as play. Use server result validation, abuse monitoring and clear review rules. Public matchmaking and friend matches need explicitly defined rating/reward policies.

### Offline coins and economy integrity
A fully disconnected client cannot be trusted to report unlimited coin rewards into an online stake wallet. Recommendation: permit offline play freely, but keep unverified local rewards separate from spendable online coins. If offline performance must earn online-eligible coins, define a reconnect-and-verification path, reward caps and anti-abuse design; verification requires more than accepting a client-submitted win. Disclose which balances are eligible for online entries.

Model coin sources, daily reward caps, repeated-bout farming, win rates and sinks before setting earnings. At a symmetric 50% win rate, expected online balance change per player per match is minus 5% of entry: 0.5*(0.9*entry) + 0.5*(-entry). The retained pot therefore drains the player economy unless rewards or other sources replenish it. Do not assume every player can continually progress to higher tiers.

### Open product decision: what can coins do outside matches?
Coin purchases are now requested (see Section 19). Not specified: starting grant amount; earning schedule; transfers; redemption; cash withdrawal; real-world prizes; age/territory eligibility. For now, make no assumption that coins have cash value. Purchase or redemption features require a separate jurisdiction-specific and platform-policy research workstream before implementation commitments. The previous budgets do not include a validated wallet/economy/abuse-operations plan for this expanded scope.

### Additional release gates
Verify insufficient-funds handling, concurrent reservations, duplicate settlement, draw/refund, reconnect, forfeits, cancelled invitations, fraudulent result submissions, altered local saves and friend collusion. Test honest presence counts and empty queues. Test all six tier amounts from versioned server configuration. Confirm slow-motion review happens after online rounds/matches without changing either participant's live simulation speed.

## 19. Player profiles, progression, purchases and rankings
User requirements added 6 October 2026. Features are specified for planning, not implemented. This section supersedes Section 18's unresolved purchase requirement. Reward amounts, repetition rules and geographic scope remain open.

### Profile and rewards
| Feature | Requested behavior | Proposed implementation |
|---|---|---|
| Victory stars | Add a star whenever a player wins | One star per verified win; separate Offline and Online star totals |
| Free coin allocation | Give the player free coins | Proposed one-time starter grant; exact amount and any recurring grants unconfirmed |
| Five consecutive wins | Award a coin bonus after five straight wins | Proposed non-overlapping streaks of five eligible wins; loss resets; draw policy must be defined |
| Fifty games played | Award a participation bonus | Count completed eligible matches; first-time versus recurring milestone unconfirmed |
| One hour on platform | Award a time-based bonus | Recommend cumulative active play/training time rather than idle login; reset/recurrence unconfirmed |
| Coin purchases | Allow purchasing coins when balance is insufficient | Optional shop with clearly priced packs; purchase never starts a match automatically |
| Local and global rank | Show regional and worldwide standing | Separate online skill rating from stars and wallet balance |

A victory grants the star and any configured win reward. For Online, the existing tier payout remains separate from any additional free win bonus; do not accidentally pay the entry-funded prize twice. Ordinary win-bonus amounts are not set. For every proposed grant display the amount and reason in transaction history. Stars are achievement counters, not currency or a skill rating; high-volume easy AI wins must not inflate Online rank.

Define eligibility separately for Offline, verified Online public matches, friend matches and training. Recommendation: competitive stars/rating require verified online results; Offline stars remain available locally. Coins intended for online stakes require server verification as described in Section 18. Repeated friend wins need explicit bonus restrictions to prevent arranged reward farming. Short abandoned games and inactive sessions should not count toward participation rewards.

Use server-configured reward values and versioned policies. Each grant has a unique event/milestone key and is issued at most once. Example keys: account/start-grant, match/win-bonus, account/season/fifty-games, account/day/active-hour. These examples do not establish daily or seasonal cadence. Specify whether streaks reward wins 5, 10, 15, or every win after 5; the recommended starting rule is non-overlapping five-win blocks. Persist progress across sessions. Disconnection, draw, forfeit, reversed-cheating results and policy changes require documented handling. Test overlapping triggers when the same win completes multiple milestones.

For an hour reward, increment eligible activity using validated match/training state and interactions. Merely leaving the app open is easy to farm; disclose the rule to users before launch. A 50-game counter encourages participation, but daily emission caps, minimum legitimate completion criteria and abuse review are needed if its coins can enter competitive pots.

### Purchases and wallet
Shop packs must show coin quantity, real price/currency and receipt history. Pack prices are open. Verify payment receipts/webhooks on the server, grant once, reconcile pending transactions and handle refunds/chargebacks under published terms. A modified client cannot mint purchased coins. Track purchased, promotional and won coin origins internally even when presenting one total wallet balance. Decide spend order and whether each origin can enter matches. Preserve a no-purchase route through Offline play and free rewards; do not auto-charge when coins run out.

Purchase eligibility does not settle cash withdrawal, transfers or redemption. Do not promise any real-world value. Platform and jurisdiction review must evaluate the actual coin lifecycle and target markets. Apple prohibits in-app purchase currency for use with real-money gaming [S16]. Google Play generally prohibits staking money or purchased items for prizes of real-world monetary value outside specified permitted categories [S17]. These policies do not by themselves establish whether this proposed closed virtual-coin system is permitted; no cash-out has been specified. Resolve that classification before implementing live payments or promising store approval.

### Local/global rankings
Recommended ranking basis: an online skill rating updated from verified results and opponent strength. Wins, losses, draws, streak, star totals and coins appear as separate statistics. Paid coins must not buy rating points or competitive stat advantages. Coin stake tier and skill division remain separate concepts.

Show Global rank and a labeled local rank such as Country rank; optionally add region/city later. Country is a proposed initial scope, not yet chosen by the user. Offer geographic privacy controls; never publish exact coordinates or street address. Prefer a stable home competitive region over automatic changes every time the player travels. If the desired rule is current location, define coarse region verification, refresh frequency, VPN/travel handling and change cooldowns. Do not assume phone GPS is available or required for a PC product.

Leaderboard entries: rank, display name, region badge where opted in, rating, online stars and eligible match count. Profile includes a personal rank even outside the visible top list, last refresh time, provisional/unranked status and seasonal/lifetime views if introduced. Geography-hidden accounts can remain on the global board. Define minimum placement matches, tie ordering, inactivity rules, season resets and result reversals before launch. Do not combine Offline AI results with Online competitive standings.

### Economy and verification gates
Daily emitted coins = starter grants + ordinary win bonuses + streak bonuses + participation bonuses + time bonuses. Entry-pot retention removes coins; paid pack grants add purchased coins. Simulate activity, win distributions, reward farming and inflation before selecting amounts. The earlier 5% per-entry expected loss in symmetric online matches excludes these added bonuses; the overall wallet expectation changes when grants are included.

Release tests: one star per verified win; separated Offline/Online counters; no bonus on duplicate events; five-win boundary/reset behavior; fifty-game eligibility; cumulative active-hour boundaries; concurrent milestone claims; receipt replay; payment failure/refund reconciliation; regional changes; correct global/local rank; rating independent of purchases; and revoked fraudulent results. New backend services include profile, rewards, wallet, commerce, geographic eligibility and leaderboards alongside matchmaking and combat hosting. Reestimate cost and operations before committing to production scope.

## 20. Fighting Clubs — community, chat and free coins
User requirement added 6 October 2026. Add a Fighting Clubs section with Create, Search and Join. Clubs let players connect, chat, invite each other to fights and receive configured free-coin rewards. This is a product specification, not an implemented feature. Coin amounts and reward cadence remain undecided.

### User flows
- Search: find clubs by name/tag; filter by language, broad region, membership availability and beginner-friendly status. Show member count, description, joining policy and activity summary using real data.
- Join: preview club rules and rewards, then join an open club or request admission to an approval-only club. Invitation-only clubs require an invitation. These admission options are proposed defaults, not user-selected requirements.
- Create: choose a unique name/tag, original badge, description, language/region and joining policy. Validate names and imagery, appoint an owner and publish the club. Creation cost, minimum player level and member limits are open; do not assume a coin fee.
- Connect: inspect member profiles, see privacy-respecting availability and invite a consenting member to a friend match. Membership or a chat invitation never authorizes a match entry debit.
- Chat: provide a persistent club channel with text, timestamps and basic notifications. Voice chat and private messaging are possible later additions, not initial requirements.

Proposed navigation: Clubs → My Club / Search Clubs / Create Club. Club page: Overview / Members / Chat / Rewards. Show the club badge on player profiles where opted in. Display reward progress and eligibility in Rewards rather than hiding conditions in fine print.

### Membership and permissions
Recommendation: one active club membership per account at launch; users can leave and join another under disclosed cooldown/reward rules. Roles: owner, moderator and member. Owner manages club settings and appoints moderators; moderators manage admissions and chat moderation; members chat and invite. Define owner transfer before leaving, inactivity succession, kicks, bans and club deletion. Member limits and cooldown durations require a product decision.

Club membership is an Online feature. Previously downloaded club content may be shown while offline with a clear last-updated label, but joining, posting and claiming rewards require the server. Do not misrepresent cached members as currently online.

### Free coins
Requested: club membership offers free coins. Recommended starting design: one account-wide first-club joining bonus plus limited rewards for meaningful participation. Exact amounts, milestones and recurrence are open. A player who leaves, rejoins, switches clubs or creates a new club must not receive the first-join grant again. Distinguish an immediate joining grant from rewards that require completed eligible activity; communicate each rule clearly.

Do not award coins per chat message: this encourages spam. Potential eligible activities include verified completed member matches or structured club challenges, subject to the friend-match collusion rules in Section 18. Do not give club leaders unlimited minting authority. Grants come from configured system reward policies and the same authoritative wallet ledger; each has an idempotent event key. No shared club treasury, peer coin transfers, paid invitations or cash redemption is introduced by this requirement.

Include club grants in economy emission simulations. If a per-player reward is offered for a shared milestone, define eligible membership at the time of achievement and at claim time, reward caps and how late joins/kicks are handled. Separate member-level eligibility from club-wide progress.

### Chat and community operations
Provide report, block/mute, spam throttling and moderator controls. Blocking prevents unwanted direct contact without silently granting moderation powers. Authenticate membership and role checks server-side for reading/posting restricted content, joining, kicking and rewards. Establish retention/deletion policies, moderation records, appeals and appropriate age/privacy settings before launch. Do not expose precise location or contact details through club search. Moderation needs ongoing staffing or an explicit support plan; a chat feature is not finished when messages can be sent.

### Backend and validation
Add Club, ClubMembership, ClubRole, ClubInvite/JoinRequest, ClubMessage and ClubRewardProgress records with stable IDs, timestamps and audit events. Chat delivery should tolerate reconnects without duplicating posts; presence should expire stale sessions. Club changes and reward issuance require consistent server state.

Release checks: search/open/approval/invite flows; duplicate/concurrent joins; one-membership enforcement; owner transfer; permission bypass attempts; kicks/bans; blocked-user interaction; chat spam; report handling; repeated join bonus prevention; reward double-claim; collusive matches; privacy settings; accurate member counts and expired presence. Reestimate backend, moderation and support costs for this scope addition.

## 21. Realistic presentation and research-driven real fighters
User requirement added 6 October 2026: the game should look very real and represent many popular fighters, including Mike Tyson, through serious research into their fighting styles and characteristics. Treat recognizable movement, tactical behavior and physical appearance as separate acceptance requirements. The target is convincing, evidence-supported representation; perfect reproduction of a human fighter cannot be guaranteed.

### Research status and roster strategy
The source review below is an initial research foundation, not completed fight-film analysis. No full bouts have been annotated, no fighter measurements have been independently verified, and no coach or rights holder has approved a representation. Do not label these preliminary profiles production-ready.

Start detailed research with Tyson and Ali to test contrasting heavyweight behavior. Expand the research queue to Joe Frazier, George Foreman, Lennox Lewis, Evander Holyfield, Floyd Mayweather Jr., Manny Pacquiao, Roberto Duran, Sugar Ray Leonard, Marvin Hagler, Thomas Hearns, Canelo Alvarez, Oleksandr Usyk, Terence Crawford and Naoya Inoue. This is a proposed research roster, not a licensed launch roster. Select eras and weight classes before comparing athletes. Cross-weight fantasy bouts need an explicitly labeled normalization mode with separate tuning; do not silently give a lightweight heavyweight dimensions or imply historical accuracy.

### Initial evidence and implementation questions
| Fighter | Initial documented foundation | Proposed representation to investigate | Evidence still needed |
|---|---|---|---|
| Mike Tyson | The International Boxing Hall of Fame documents Cus D'Amato's role in developing Tyson and the trainer lineage that included Kevin Rooney [S18] | Begin with a proposed 1986–1988 heavyweight profile. Investigate compact guard, slipping during entry, explosive distance closing, angle changes and head/body combinations. These are research hypotheses here, not measured parameters | Full-bout timestamps supporting each action, entry failures, guard recovery, later-round behavior, and differences from later Tyson |
| Muhammad Ali | The Hall of Fame describes exceptional speed, jab-led boxing, low lead-hand positioning and pull-back defense, and separately his rope-a-dope strategy against Foreman [S19] | Separate a proposed 1964–1967 mobile profile from a 1974 profile. Author movement, jab and defense transitions for each rather than combining every celebrated tactic into one permanent behavior | Footwork cadence, opponent-dependent tactics, rope behavior and fatigue observations from complete bouts |
| Roberto Duran | The Hall of Fame describes a relentless style and a career spanning multiple championship weight divisions [S20] | Study lightweight and welterweight versions separately; investigate close-range control, defensive positioning and attack construction | Detailed inside-work annotations; the short biography alone does not establish specific mechanics |
| Sugar Ray Leonard | His foundation describes speed and power, while his career background provides a starting point for selecting footage [S21] | Investigate transitions between movement and combination attacks, changing tactics against different opponents | Bout-level evidence for attack rhythm, range changes and defensive recovery |
| Floyd Mayweather Jr. | Trainer Calvin Ford describes differences between Mayweather's younger inside-fighting approach and later selective punching [S22] | Separate early-career and later-career profiles; investigate context-dependent guard choices and counters | Direct bout annotations; a trainer interview does not establish exact action probabilities |
| James Toney | The Hall of Fame explicitly identifies counterpunching, defense and his signature shoulder roll [S23] | Use as an additional defensive research reference; investigate counters and shoulder-roll transitions under different attacks | Contact geometry, foot placement, body defense and failure cases |

The proposed mechanics column is a development interpretation or research question. Source descriptions do not establish animation timings, punch force, stamina curves, AI ratings or win probabilities. Avoid converting reputation into arbitrary maximum stats.

### Required dossier for every fighter and era
Each dossier must include:

- Identity and physical reference: era, division, age at reference bouts, stance, sourced height/reach and bout weight, proportions, facial/body references and equipment. Record conflicting measurements rather than silently choosing one.
- Movement: stance width, balance, guard positions, head rhythm, steps, pivots, direction changes, ring cutting, retreats and behavior near ropes/corners.
- Offense: punch paths, setup cues, target selection, combinations, feints, preferred distance, entry and exit patterns, recovery and missed-punch behavior.
- Defense: blocks, parries, slips, rolls, pull-backs, counters, clinch tendencies and openings. Describe the conditions where defenses fail as well as succeed.
- Tactics: responses to opponent stance, height, reach, pressure and pace; leading versus countering; changes by round, fatigue and score situation. Model observable decisions rather than inferred private thoughts.
- Presentation: era-specific face/body, gloves, trunks, boots, idle motion, breathing, sweat and reactions. Public persona, voice and entrances require their own evidence and asset permissions; biography must not become an unsupported psychological rating.
- Traceability: source URL or archive ID, bout/date/opponent, video edition, round/timecode, observation, confidence, annotator, disagreement notes, rights status and linked animation/AI rule.

### Full-bout study and annotation method
Proposed starting sample per era: 6–10 complete bouts, chosen for varied opponents and circumstances; increase the sample when patterns remain uncertain. Include competitive fights, losses or difficult rounds where available, early and late rounds, different opponent stances, and both successful and failed techniques. Highlights may locate examples but cannot establish representative frequencies.

Use official broadcaster/promoter archives, authorized athlete/trainer material, documented records and credible historical sources. Maintain a footage inventory with access and reuse status. Public availability is not permission to embed, redistribute or train on footage. Follow the rights workflow in Section 13 before production use.

Annotate exchanges with neutral labels: starting range/angle, stance, visible cue, action sequence, foot placement, guard changes, contact/block/miss, recovery, opponent response and ring position. Mark occluded or ambiguous events unknown. Use timestamps relative to a identified video edition; broadcast cuts and slow-motion inserts must not be mistaken for real action duration. Monocular footage does not reliably establish impact force or exact three-dimensional motion.

Have two analysts independently label a shared sample and a qualified boxing coach resolve material disagreements. Calculate action frequencies with denominators and context, such as counters per observed eligible attack, rather than raw highlight counts. Record uncertainty and selection bias. Reserve several bouts as validation material so tuning and assessment do not use only the same examples.

### From research to animation and Fighter DNA
Map each accepted finding into versioned content: a movement/technique asset, a conditional tactical preference, a physical constraint or a presentation reference. Keep the original evidence attached. A signature combination needs its setup, footwork, contact, miss, interruption and recovery variants; one cinematic montage is insufficient.

Use authorized scanning/reference work for likeness, then boxing-specialist motion capture and manual animation cleanup for mechanics. A performer should reproduce reviewed movement safely; resemblance alone does not make their technique accurate. Validate shoulders, hips, weight transfer, planted feet and glove trajectories at normal speed and in slow motion. Video-derived motion is a candidate reference requiring correction, not automatically ground truth.

AI chooses through the shared combat API using observations available to that fighter. Represent preferences as conditional distributions: range, opponent cue, ring position and resource state affect the choice. Preserve plausible alternatives and mistakes. In player-controlled mode, fighter mechanics and capabilities remain distinctive while the player decides tactics; AI/watch mode additionally applies the researched tactical policy. Difficulty must not erase style by granting perfect reactions.

Add clinches, advanced rope interactions, southpaw or stance switching only when the simulation supports them. A profile with missing essential mechanics remains an incomplete representation. If prototype scope excludes those mechanics, explicitly label that limitation rather than claiming full authenticity.

### Visual realism and validation gates
Build a reference scene with two fighters and one ring before expanding the roster. Establish consistent scale, body proportions, anatomically credible deformation, skin/glove materials, lighting, shadows, cloth, sweat, camera and sound. Benchmark gameplay cameras and close-up replay separately on a declared PC hardware tier. Prioritize stable movement and visible contact agreement alongside visual fidelity.

For each fighter, require a coach-reviewed animation coverage matrix, evidence-linked tactical rules and tests against scripted styles: retreating jabber, aggressive pressure, guarded counterpuncher and opposite stance where supported. Compare simulated behavior with held-out footage distributions under comparable situations, acknowledging that live opponents and match circumstances differ.

Run blinded coach/fan recognition checks using neutral clothing and anonymized movement to test whether style is recognizable beyond face and branding. Separately assess appearance, movement credibility, tactical identity and contact correctness. Define numerical pass thresholds after the pilot establishes workable baselines; no accuracy percentage is claimed here. Historical bout outcomes are context, not a requirement that simulations reproduce a predetermined winner.

### Deliverables and production gate
For every roster candidate deliver a source register, era dossier, annotated exchange dataset, signature-action coverage matrix, Fighter DNA configuration, animation reference pack, review record and validation report. Track status as proposed, sources collected, annotated, coach reviewed, implemented, validated and rights cleared; do not collapse those stages into one approval.

First research milestone: complete Tyson's selected-era dossier and a contrasting Ali dossier, then prove their differences in a small movement/combat prototype. Expand only after the research-to-runtime process works. Add a boxing research lead, footage analysts, coach review and per-fighter animation work to the staffing/cost estimate; the earlier budgets do not establish the cost of a large accurately researched licensed roster. Gameplay prototyping may continue with original fighters while real-fighter dossiers and permissions develop.

## 22. Fighter roster and competitive achievement groups
User requirement added 6 October 2026: list different real fighters, research their characteristics deeply and group them by level of professionalism. The initial 24-fighter source review and dossier briefs are in [fighter_research.md](fighter_research.md).

Interpret levels as competitive achievement within a division/era, not personal conduct. Proposed bands are established legends, accomplished world champions, leading world contenders and developing/regional professionals. These are editorial roster groups, not official rankings. Keep weight class, era, style, opponent difficulty, online skill rating and coin stakes separate. No regional/developing names are assigned without individual research.

The companion document includes named groups, career evidence, tactical study questions, AI hypotheses, suggested bout queues, source limitations and deep-research completion requirements. The 24 entries comprise 13 established-legend candidates, six accomplished world champions, two leading contenders and three additional elite women's championship candidates awaiting the same historical grouping review. Some boundaries, especially Bowe, Toney and McCallum, are explicitly open to review.

The source review is completed for this edition; full-bout annotation and coach validation remain outstanding. A roster brief must not be presented as a finished motion/behavior model. Expand the roster and finalize groups only as evidence and era-specific dossiers mature.

## 23. Confirmed live professional fight experience
User confirmed this vision on 6 October 2026. The product must make spectators feel they are watching a credible live boxing event while giving players responsive control. This is the central production target, not an optional graphics upgrade. This section expands the earlier prototype scope with an integrated presentation milestone; the existing small combat proof remains an earlier engineering step.

### Complete event sequence
Support event setup and fighter selection; arena establishing shots; fighter entrances; ring introductions; referee instructions; opening bell; rounds and exchanges; between-round corner activity; knockdown/count and stoppage handling where applicable; final bell or stoppage; result announcement; celebration and post-fight highlights. Let players skip nonessential sequences. Exact entrance choreography, announcer scripts and commentary scope are content decisions rather than already produced assets.

### Core presentation requirements
| Area | Required experience | Implementation and review requirement |
|---|---|---|
| Fighters | Credible faces, bodies, equipment and recognizable movement | Era-specific references, natural deformation and coach-reviewed movement; follow real-fighter dossier and rights gates |
| Combat | Footwork, punches, defense and reactions agree visually | Authoritative contact aligns with visible glove/guard positions; believable misses, recovery, balance and fatigue |
| Arena | A convincing professional venue | Ring, ropes, canvas, lighting, ringside personnel and varied crowd behavior; profile gameplay and replay cameras |
| Broadcast | Watch mode resembles a televised fight | Establishing and ringside views, readable round/clock/score graphics, contextual replay and highlights; protect gameplay readability and input control |
| Audio | The audience can hear and feel the fight | Bell, footsteps, gloves, breathing, crowd and arena ambience with a balanced mix; intensity follows real match events |
| Commentary | Calls reflect what actually happened | Consume verified combat events, manage repetition and avoid invented punches, injuries or unsupported historical claims |
| Crowd | Reactions evolve with the bout | Responses to pressure, clean exchanges, knockdowns, recoveries and decisions; variation rather than a permanently loud loop |
| Officials | Rules and visual decisions agree | Referee behavior, counts, round transitions and stoppages consume authoritative rule state; scoring follows the selected game rules |
| Corners | Round breaks feel like part of a fight | Corner personnel, breathing/recovery animation and contextual advice based on recorded events; avoid unsupported physical diagnoses |
| Drama | Excitement emerges from the contest | Momentum changes, rivalries where authored, reactions, celebrations and highlights; competitive outcomes follow simulation and verified results |

Entrances, introductions, commentary, venue branding and fighter presentation require their own approved assets and content coverage. Build an original event identity for the initial prototype. Detail such as sweat, bruising, cloth and facial reactions must stay coherent with animation, performance and supported consequence states. Detailed cuts and medical assessments remain outside the earlier first combat proof.

### Play mode and Watch mode
Both modes consume the same combat and event state. Play mode needs a stable, readable camera, responsive input and optional assists. Watch mode can use directed camera changes and broadcast pacing. Replays may select alternate views and slow motion while preserving the recorded outcome. Live online play retains the timing constraints in Section 18. Broadcast camera cuts must not hide information essential to a player's control.

### First integrated vertical slice
Deliver two original, visually credible fighters with contrasting researched style principles, one polished arena, controller-based play and AI-versus-AI Watch mode. Include a short multi-round bout, entrances and introductions, bell and round graphics, reactive crowd/audio, basic contextual commentary, corner breaks, a supported knockdown/count or stoppage sequence, result announcement and a replay highlight. This is larger than the initial debug combat prototype and must be estimated separately.

Provide configurable development scenarios to inspect round breaks, knockdowns and results without claiming those outcomes occur in every ordinary bout. Establish the selected rule profile and scoring behavior before presenting a decision. Keep authentic celebrity fighters as a subsequent milestone once dossiers, motion coverage and permissions are ready.

### Acceptance and delivery gates
Review complete bouts at normal speed before inspecting close-up replay. Assess fighter movement, physical contact, camera continuity, event pacing, audio coherence and responsive controls separately. Verify that commentary, crowd triggers, overlays, referee actions and highlights match recorded events. Review with boxing coaches and viewers familiar with professional broadcasts; record specific shortcomings instead of relying on a general 'looks real' judgment.

Declare the PC hardware tier and profile a full event, including crowd, entrances and replay. Retain the proposed 60 FPS gameplay target from Section 3; actual feasibility requires measured results. Validate stable frame times and input behavior, not only attractive still images. Set reviewer thresholds and performance budgets during the slice pilot rather than claiming a proven realism percentage.

Production sequence: combat/movement proof, integrated event slice, fighter research and content expansion, then online/economy/community integration against the established combat foundation. Add broadcast direction, environment/crowd art, audio, commentary writing/recording and event animation to staffing and costs. The earlier estimates are not a commitment for this expanded presentation scope. No game implementation or completed live-event assets exist yet in this workspace.

## 24. Pre-implementation gap review
Review completed 6 October 2026. See [implementation_readiness.md](implementation_readiness.md) for missing specifications, supporting primary documentation, concrete deliverables and milestone gates.

The most important additions are exact controls and input timing; ownership of movement/root motion; ring/body interactions; professional scoring; complete knockdown/stoppage transitions; clinch/rope dependency research; broadcast camera direction; commentary coverage; crowd performance budgets; accessibility; saves/recovery; content production throughput; and concrete online operations/account/network decisions.

The project can begin focused original-fighter combat experiments after documenting their small scope. It is not yet ready for a full-production commitment: no engine build, measured hardware results, completed film annotation or validated content-cost baseline exists. Complete the relevant readiness gate before implementing each dependent feature; do not require every commercial/online question to be settled before a reversible offline proof.

Distinguish three scopes consistently: the small contact/movement proof in Sections 3/15/17; the complete live-event slice in Section 23; and subsequent commercial/real-fighter/online expansion. Extra ideas such as career mode and tournaments remain optional rather than silently expanding the confirmed scope.

## 25. Mobile and Web structure
The platform architecture proposal is documented in [platform_structure.md](platform_structure.md). It separates native mobile gameplay, a responsive web companion, locally rendered browser gameplay and cloud-streamed gameplay. These alternatives must not be treated as interchangeable exports of one website.

Use one authoritative backend for accounts, profiles, clubs, rankings, rewards and wallet/commerce, plus an online combat host. Share versioned content and interface contracts; adapt controls, UI, rendering, storage and payment integrations per platform. The proposed repository layout and feature ownership table define these boundaries.

The user confirmed Mobile game plus Web companion on 6 October 2026. Playable fights, offline AI, interactive Academy drills and AI Watch run in the native mobile game. The responsive Web companion supplies account/community interfaces and reviewed lesson/replay media through shared services. Local mobile simulation preserves disconnected offline play. Browser combat and cloud streaming are future alternatives only; the confirmed arrangement supersedes earlier PC-only public-launch assumptions.

## Sources
Official documentation, rights material, research and product claims consulted on 6 October 2026. Vendor feature lists describe capability/marketing, not independent quality measurements. Source-linked summaries are intentionally brief; the detailed project design above is original planning analysis.

[S1] Undisputed, official features: https://www.playundisputed.com/features
[S2] Epic, Motion Matching: https://dev.epicgames.com/documentation/unreal-engine/motion-matching-in-unreal-engine
[S3] Epic, Physics-Based Animation: https://dev.epicgames.com/documentation/unreal-engine/physics-driven-animation-in-unreal-engine
[S4] Epic, MetaHuman scalability: https://dev.epicgames.com/documentation/metahuman/performance-and-scalability-settings-for-metahumans
[S5] Epic, Replay System and streamers: https://dev.epicgames.com/documentation/unreal-engine/using-the-replay-system-in-unreal-engine ; https://dev.epicgames.com/documentation/en-us/unreal-engine/demonetdriver-and-streamers-in-unreal-engine
[S6] Unity, 2026 pricing: https://unity.com/products/pricing-updates
[S7] World Boxing, competition resources: https://worldboxing.org/competitions/
[S8] Epic, StateTree: https://dev.epicgames.com/documentation/en-us/unreal-engine/state-tree-in-unreal-engine
[S9] Peng et al., AMP (2021): https://arxiv.org/abs/2104.02180
[S10] England Boxing, coaching handbooks: https://www.englandboxing.org/news_articles/level-1-and-level-2-coaching-course-handbooks-now-available-online/
[S11] Google, Pose Landmarker: https://developers.google.com/edge/mediapipe/solutions/vision/pose_landmarker
[S12] California Civil Code 3344 reproduction, including 2026-effective amendment: https://law.justia.com/codes/california/code-civ/division-4/part-1/title-2/chapter-2/article-3/section-3344/ ; Assembly committee analysis discussing publicity/digital-replica provisions: https://apcp.assembly.ca.gov/media/994
[S13] Epic licensing and EULA: https://www.unrealengine.com/license ; https://www.unrealengine.com/eula/unreal
[S14] Steamworks onboarding: https://partner.steamgames.com/steamdirect/
[S15] Epic, Networked Physics: https://dev.epicgames.com/documentation/unreal-engine/networked-physics-overview

[S16] Apple App Review Guidelines, gaming provisions: https://developer.apple.com/app-store/review/guidelines/
[S17] Google Play, Real-Money Gambling, Games and Contests: https://support.google.com/googleplay/android-developer/answer/9877032?hl=en

[S18] International Boxing Hall of Fame, Cus D'Amato: https://www.ibhof.com/pages/about/inductees/nonparticipant/damato.html
[S19] International Boxing Hall of Fame, Muhammad Ali: https://boxhall.wixsite.com/ibhof/ali
[S20] International Boxing Hall of Fame, Roberto Duran: https://boxhall.wixsite.com/ibhof/duran
[S21] Sugar Ray Leonard Foundation, career biography: https://www.sugarrayleonardfdn.org/sugar-ray-leonard
[S22] Premier Boxing Champions, trainer Calvin Ford interview: https://www.premierboxingchampions.com/news/fundamentals-calvin-ford
[S23] International Boxing Hall of Fame, James Toney (page URL retains a different template title): https://boxhall.wixsite.com/ibhof/copy-of-jones-roy
