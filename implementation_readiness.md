# Pre-implementation review

Reviewed 6 October 2026 against fight_game.md and fighter_research.md.

The vision and major feature areas are documented. The project is ready for focused engineering experiments, but not yet for a commitment to full production. Several requirements describe the desired result without specifying the behavior, content volume or measurable completion conditions. No engine project, asset benchmark or completed combat implementation exists in this workspace.

This review adds missing specifications and research tasks. Proposed defaults below are starting recommendations, not decisions already made by the user. Existing offline/online requirements remain in force, including no offline human-versus-human matches and no undisclosed bots in online matches.

## Resolve before the combat prototype

| Gap | Required specification or experiment | Concrete output | Suggested accountable role |
|---|---|---|---|
| Controls and responsiveness | Map movement, punches, head/body selection, guard, slip, pivot, feint and pause. Specify hold/toggle behavior, dead zones, buffering, cancellation and simultaneous commands. Compare simple button controls with any analog punch proposal | Versioned control map and input-to-action timing trace | Gameplay lead/designer |
| Movement ownership | Decide how simulation movement, root motion, foot planting and animation warping cooperate. Test approach, retreat, circling, blocked travel and punching while moving | Movement contract and debug scene showing feet, body and glove trajectories | Gameplay/animation leads |
| Interaction priority | Resolve two punches landing together, interrupted recovery, guard changes at contact, collision during knockdown and round-ending contacts | State-transition table and adjudicated exchange cases | Gameplay lead/coach |
| Ring boundaries and bodies | Define fighter separation, pushing, corners, rope contact and what happens when a requested action has no space. Avoid body overlap and stretched punch reach | Boundary policy and crowded/corner test cases | Gameplay/animation leads |
| Resource/consequence tuning | Specify short-term effort, fatigue, stun, balance, regeneration and recovery by action/phase. Choose how much the player can see in the HUD | Tunable prototype parameters and reason-coded outcome log | Designer/coach |
| Measurement and reproducibility | Select test hardware and display/controller configuration; record build, assets and settings. Separate render rate, simulation rate and end-to-end input delay | Benchmark manifest and repeatable contact/replay harness | Technical lead/QA |
| Project setup | Pin an evaluated engine version; establish binary asset storage, content naming, units/skeleton conventions, packaged development builds and backups | Reproducible project setup and first packaged build | Technical lead |

Epic's Enhanced Input supports remapping, dead zones and contextual actions, but it does not choose boxing controls for us. [Enhanced Input documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine)

Platform decision updated 6 October 2026: native mobile game plus web companion is confirmed in platform_structure.md. Prioritize a touch-control experiment on actual mobile devices, with optional controller support. Windows/keyboard may support development debugging but is not the launch assumption. Keep complex alternative controls as a comparative experiment rather than introduce them into all assets before testing. Final controls must be validated with players.

## Resolve before the integrated live-event slice

| Gap | Required research/specification | Concrete output |
|---|---|---|
| Professional scoring | Choose a specific rule profile and model round-level judging rather than punch totals. Research applicable scoring criteria, deductions and decision types; keep judge reasoning inspectable | Scoring design, coach-reviewed sample rounds and decision tests |
| Knockdown and stoppage sequence | Distinguish knockdown, KO, referee stoppage, corner retirement and decision. Define neutral-corner behavior, counts, readiness checks, recovery controls and bell/count ordering | Bout/referee state machine with supported/unsupported outcomes |
| Clinches and ropes | Decide the slice's support for clinch initiation, referee break, legal short punches, corner movement and rope lean. These are dependencies for several real styles | Animation coverage and interaction experiment; explicit slice limitations |
| Broadcast direction | Define camera locations/lenses, framing, occlusion handling, continuity, cut timing and which moments can interrupt Watch mode. Avoid cuts that disrupt player control | Camera style guide and complete event storyboard |
| Commentary and statistics | Define event priorities, interruption/cooldowns, pronunciation, repeated-line suppression and distinction between official results and estimated statistics | Event-to-line matrix, original sample script, subtitle data and recording estimate |
| Crowd and arena cost | Estimate crowd size/variation, LOD/update policy, lighting and audio layers. Budget entrances, referee, corners and audience together | Profiled representative scene with CPU/GPU/memory/streaming budgets |
| Corners and recovery | Define the gameplay effect of breaks, recovery limits and advice from recorded evidence; decide whether corner interaction is cinematic or player-directed | Corner transition specification and contextual advice examples |
| Presentation of consequences | Define supported sweat, bruising, swelling and equipment motion. Specify when effects appear and persist; maintain coherence across camera cuts/replays | Visual state model and reference assets; detailed cuts remain expansion work |
| User interface and onboarding | Define selection, rule/difficulty explanations, fight HUD, menus, post-fight scorecards and replay controls; test first-time completion | Clickable or in-engine flow prototype and usability findings |
| Accessibility and comfort | Provide remapping, scalable text, subtitles/speaker labels, separate audio sliders, non-color-only information and adjustable shake/blur. Avoid mandatory button-mashing recovery | Settings specification and accessibility playtest checklist |
| Saves and interruptions | Define offline progress/settings saves, crash-safe writes, corrupt-save recovery, controller disconnect handling and replay compatibility. Decide whether mid-bout resume is supported | Persistence schema, migration policy and interruption tests |
| Content throughput | Measure the complete cost of one fighter, one signature sequence and one polished event segment, including revision and optimization | Asset inventory, supplier quotes where relevant and revised slice estimate |

The ABC publishes a professional boxing rule baseline with specific knockdown, bell and foul provisions; those choices differ from casually assumed game rules. Select and label the applicable profile rather than assume universal rules. This review does not reproduce or certify a complete jurisdictional rulebook. [ABC Unified Boxing Rules](https://www.abcboxing.com/unified-rules-boxing/)

Steam's accessibility guidance covers configurable audio, subtitles, text size, color alternatives, camera comfort and input options. Only claim features that the delivered build actually supports. [Steamworks accessibility guidance](https://partner.steamgames.com/doc/accessibility_features?l=english&language=english)

Epic's Animation Budget Allocator can reduce animation work for many characters, but this is a candidate crowd optimization, not a benchmark result. Critical fighter motion/contact sampling must not be degraded by a crowd optimization policy. [Animation Budget Allocator](https://dev.epicgames.com/documentation/unreal-engine/animation-budget-allocator-in-unreal-engine)

Suggested slice default: original arena branding, original commentary and original fighters; one labeled simplified professional rule profile; supported count/stoppage/decision paths; configurable display/comfort settings. Decide recovery controls and clinch/rope scope through the prototype. Do not imply an authentic real-fighter implementation while essential mechanics are missing.

## Research required before real-fighter production

- Verify physical reference, stance, division, era and title chronology with independent sources. The current 24 briefs are starting research, not full behavioral models.
- Complete full-bout annotation and coach review for each selected launch version. Include defensive failures, misses, later rounds, different opponent styles and held-out validation footage.
- Research fighter-specific idle motion, gait, defense-to-attack transitions, reaction/recovery and equipment alongside signature combinations. Similar skeletal rigs do not imply identical movement.
- Establish use rights and reference provenance for footage, likeness, voice, tattoos, music, venues and branding using the existing rights register. Track whether assets remain usable after an agreement expires.
- Pilot the capture/cleanup process before booking a large roster. Estimate transitions and misses, not just individual punches; track revision hours and rights-review turnaround.
- Review achievement grouping with a boxing historian/coach. Keep disputed boundaries visible, complete expansion dossiers and classify elite women's candidates by the same achievement criteria. Avoid treating editorial groups as physical simulation coefficients.

## Resolve before online and commerce implementation

These systems already appear in Sections 18–20, but the following concrete decisions remain open:

| Open area | Required decision/evidence |
|---|---|
| Accounts and identity | Guest offline use, account provider, recovery, linked-device behavior and display-name changes; separate authentication from public profile |
| Network architecture | Compare server-authoritative movement/contact options using the actual combat prototype; define prediction/reconciliation and result validation rather than assuming the engine supplies fair boxing |
| Network acceptance | Define tested regions, latency/jitter/loss envelope, hit-disagreement criteria, reconnect behavior and server cost per active bout |
| Match conditions | Allowed fighters/eras, same-weight restrictions, normalized fantasy matches, assist policies and configuration/version compatibility |
| Settlement and rewards | Final draw/forfeit/disconnect rules, bonus amounts/cadence, offline eligibility and retained-coin destination; these remain proposed or unresolved |
| Commerce classification | Decide the actual coin lifecycle, target markets, ages and platforms before implementing live payments; specialist review uses the existing purchase-policy workstream |
| Security and abuse | Threat model for modified inputs, result fraud, receipt replay, collusion, reward farming and role bypass; documented evidence/review and account recovery |
| Operations | Service monitoring, backups and restoration drills, moderation/support ownership, deployment rollback, configuration migrations and incident handling |
| Compatibility | Engine/content/replay version handling, maintenance messaging and behavior when an update interrupts an active match |
| Player information | Clear connection state, terms, match stake confirmation, purchase receipts and accessible dispute/support flow |

Epic documents character movement prediction/correction mechanisms; these are engineering tools, not proof that custom combat contact or wallet settlement is correct. [Networked character movement](https://dev.epicgames.com/documentation/en-us/unreal-engine/understanding-networked-movement-in-the-character-movement-component-for-unreal-engine)

## Optional additions requiring a product decision

Career/story mode, tournaments, belts, custom fighters, rivalries, ranked seasons, online spectator rooms, broadcast overlays/export, mod support, voice/private chat, cross-platform play, VR/mobile and physical camera coaching are possible expansions. They are not automatically added to the implementation scope by this review. Offline AI Watch mode is already required; live human-match spectators need a separate design, including delayed information where appropriate.

## Release preparation

Before public release, specify installation/update size, supported hardware/controllers, graphics presets, loading targets, language coverage, offline behavior, save upgrades, replay storage limits and customer support. Evaluate age rating and distribution requirements for the actual content/coin features. Establish consent and retention for any diagnostic data collected and avoid collecting video or precise location by default. Recheck applicable policies against the selected markets/platforms rather than infer approval from this document.

## Milestone gates and next work

| Milestone | Done when | Current status |
|---|---|---|
| Combat proof | Movement/control contract exists; jab hits, misses and blocks visibly agree with resolution; scripted exchanges can be replayed consistently | Not started |
| Live-event slice | Short complete bout with two original fighters, event presentation, supported official outcomes, first-time playtest and measured hardware results | Not started |
| Real-fighter production | Selected-era dossiers, reviewed motion/tactics, validation and asset permissions complete | Initial source briefs only |
| Online competitive play | Network/adjudication tests and authoritative settlement/reward cases pass under defined conditions | Specification only |
| Commercial release | Budget/content capacity, release checks, operations and product validations support the committed scope | Not ready |

Begin with the control/movement contract, reference hardware manifest, supported rule/outcome table and a packaged Unreal contact prototype. Research and original-asset experiments can run alongside that work. Missing payments, celebrity permissions or complete club operations do not prevent an original-fighter offline engineering proof; they remain gates for their dependent features.

The most consequential open product choices are available budget/team, target test PC, preferred control feel, selected professional rules, launch roster size/eras and the purchase/reward lifecycle. Record decisions with owner, date, rationale and affected milestone. Do not silently turn proposed defaults into confirmed requirements.
