# Pokémon Aster — production roadmap to a finished game

Status: release-level planning v0.1. This is a dependency roadmap with deliverables and acceptance gates. It is not a dated promise or a file-by-file implementation plan. Derive detailed plans from reviewed specs one subsystem at a time.

## 1. Build order

Foundation → overworld and input → battle/capture/growth → first-gym slice → shared campaign systems → chapters 2–4 → chapters 5–8 and League → postgame → hardening and release.

Art and content expansion begin only when contracts are stable enough to avoid recreating hundreds of records. The browser and desktop save/asset probe happens early, so desktop is not left as an untested final wrapper.

| Stage | Deliverable | Exit gate |
| --- | --- | --- |
| P0 — Design and production lock | Reviewed game/UI/content/technical documents; chosen asset sources; agreed roster criteria | Owner reviews proposed defaults; launch scope and acquisition plan are credible |
| P1 — Platform foundation | Verified pinned runtime; title/profile skeleton; save format; content schemas; browser/desktop probe | Same minimal scene runs on both targets, saves/reloads, and exports/imports a profile |
| P2 — Overworld play | First village/route, collision, NPCs, dialogue, warps, interaction, input contexts | New player explores without stuck movement, invalid spawns, or repeating one-time rewards |
| P3 — Battle and Pokémon domain | Seeded singles core, catching, party, bag, EXP, learning, evolution, storage | Supported slice mechanics validated; capture/reward transactions and save recovery pass |
| P4 — First-gym vertical slice | Finished onboarding, first town/route/grove/gym, 24-species supported slice dataset, 3 quests, polished core UI | Play start-to-badge and reload; recover after defeat; pass keyboard-only and offline-desktop checks |
| P5 — Campaign toolkit | Doubles, quests, traversal, NPC trade, shops, clock, reusable puzzles, complete release mechanics contracts | Authors can add a validated chapter without new special-case engine code; lock explicit 240-species manifest |
| P6 — Campaign act I/II | Chapters2–4, route loops, 9 additional side quests, character development | Saves carry across all chapters; encounter and economy balance pass with different starter teams |
| P7 — Full campaign | Chapters5–8, story climax, League, Hall of Fame, credits; all 24 campaign side quests | A fresh profile can finish the adventure on every preset with no progression bypasses |
| P8 — Postgame | Haven areas, 8 quests, Circuit, rematches, completion and legendary retries | All 240 species obtainable in one save; postgame rewards and normalized teams are correct |
| P9 — Beta and release | Final art/audio, UX pass, performance work, old-save migration, clean installs and distribution | Completion checklist below passes; no critical progression/save bugs; release artifacts verified |

## 2. P0 decisions and concrete preparation

Review the working title/region/story, content budgets, offline single-player scope, preset rules, architecture, and first-gym look. Record accepted changes in the relevant design document, not only in chat.

Before production-scale artwork, choose a usable asset sourcing strategy for Pokémon front/back/follower sprites and cries. Record terms and attribution in a manifest. If acceptable sources are unavailable, revise the art production budget with evidence; do not schedule 240 animated creatures as if their art were free and ready.

Create the first-gym supported-species table and required effect-handler union. Separate species supported by battle data from species actually obtainable before badge1. Confirm no starter or early opponent requires an unplanned ability/evolution system.

This stage ends in detailed foundation and first-slice implementation plans; actual product development starts after design review. The owner already authorized uploading these planning documents to GitHub.

## 3. P1–P3 engineering priorities

Foundation first proves the expensive assumptions: cross-target loading, platform save adapters, portable format, UI/canvas alignment, chosen Phaser APIs, keyboard isolation, and audio startup. Use deliberately small original prototype assets; label prototype content clearly.

Then complete real world interactions and pure battle behavior. The early battle contract includes fainting, party replacement, defeat/return, PP exhaustion and Struggle, capture failure/success, reward ordering, and move/evolution dialogs. Do not leave those edge cases until gyms depend on them.

Implement only the slice's move/ability handler set initially, with validation preventing unsupported content. Add the broader release handler set before campaign content locks; an obtainable move cannot display a real description while executing a placeholder.

## 4. First-gym slice definition

A slice is an entire playable miniature chapter:

- Trainer creation and preset selection; movement/interaction teaching.
- Starter choice with real data for each candidate and the rival's advantage choice.
- First rival battle, first-route encounters, catching tutorial, optional captures.
- A Center, shop, party, bag, summaries, storage, save/export/import.
- Bellroot/first-town maps, one exploration area, hidden shortcut and pickups.
- Three side quests and their persistent progress/rewards.
- Gym map, irrigation puzzle, three gym trainers and leader, badge and field license.
- Victory, defeat, reload, return visits, and next-chapter gate.
- Representative final-style sprites, dialogue, environment, UI and music.
- Browser and desktop playthroughs, keyboard-only navigation, measured frame/load behavior.

Target experience: roughly 45–90 minutes with exploration; timing is measured, not guaranteed. Twenty-four supported species may include later-obtainable trainer species; the local obtainable count is reported accurately.

After the slice, record time spent per map, trainer, quest, sprite family, effect handler, and music asset. Estimate remaining workload from actual throughput. Only then discuss release dates, staffing, and whether particular stretch features fit.

## 5. Chapter production checklist

For each chapter, author maps/warps/collision; NPCs and stateful dialogue; preset-specific trainer teams; encounter tables and source paths; pickups and shop tier; three campaign quests; gym puzzle/teams/rewards; story milestone; traversal unlock; music/assets; optional shortcuts and revisits.

Run content validation, checkpoint save tests, defeat/retry play, and the mandatory-path run. Review available counterplay and economy. Finish one chapter coherently before several later chapters become disconnected placeholders. Record completed content in a ledger with links to actual records and test evidence.

Use consistent status: Proposed, Authored, Integrated, Verified, Release-ready. A document alone is Proposed; a map file without gameplay validation is Authored, not Verified. Counts on the README must not be updated to "implemented" until relevant gates pass.

## 6. Content workstreams and prerequisites

| Workstream | Prerequisite | Production output |
| --- | --- | --- |
| Creature data | Rule profile and schemas | 240 species with forms/abilities/evolutions and acquisition paths |
| Battle mechanics | Headless contract | Handler union covering every obtainable move/ability/item |
| World maps | Tile/collision/warp format | 11 settlements, 18 routes, 10 major area packages plus interiors |
| Quests/story | Script/quest commands | Full main path and 32 authored side quests |
| Trainer balance | Party legality and AI | 160 placed battles, rematches, 24 Circuit templates |
| Art | Style guide and sources | World sets, battle/follower sets, portraits, VFX, UI |
| Audio | Loading/buses and sources | 18 music tracks, 5 jingles, SFX and required cries |
| UI | Input/focus contract | All screens and their empty/disabled/error states |
| Release | Stable game and migrations | Browser site, Windows installer, save-compatible updates |

These are separate ownership areas, not a requirement to start every workstream simultaneously. Each depends on reviewed shared interfaces.

## 7. Beta playtest matrix

- Fresh starts with each starter and all three presets.
- Keyboard-only campaign segment, full storage/shop/summary navigation, doubles targeting, and save export/import.
- Controller disconnect/reconnect, lost window focus, resize/DPI, muted audio startup.
- League run save/resume between matches; loss and retry; postgame unlock after credits.
- Storage-full captures/gifts, no PP, full fainted party, empty inventory/money, canceled evolution.
- Quests accepted after earlier catches, rewards collected twice, old maps revisited after story changes.
- Interrupted/quota-failed writes, corrupt autosave recovery, older save migrations, incompatible imports.
- Browser pack failure/retry, compatible update, Windows offline fresh launch, clean reinstall/update.
- Completion audit for all species, quests, items, traversal gates, supported effects, and asset files.

Automate deterministic rules and persistence failures. Manually play the authored adventure for pacing and clarity. Debug shortcuts can prepare test fixtures but do not count as a valid beginning-to-credits playthrough.

## 8. Release checklist

- [ ] Campaign begins, all eight gyms work, the story resolves, the League can be won, and credits/postgame transition persists.
- [ ] All 240 regional species have verified one-save acquisition and evolution paths.
- [ ] All 32 quests complete and reward exactly once; mandatory flags survive reload.
- [ ] Postgame areas, Circuit, gym/League rematches, legendary retries, and Dex rewards are functional.
- [ ] Every obtainable move, ability, and item executes its actual documented effects.
- [ ] Every shipped sprite, map, portrait, sound, track, and icon exists with recorded provenance.
- [ ] Every major UI screen passes populated/empty/disabled/error review and keyboard navigation.
- [ ] Three profiles, manual/autosave/recovery, export/import, and old-save migration pass.
- [ ] No known critical bug, save-loss defect, or mandatory-path softlock remains.
- [ ] Performance/load targets are measured on recorded PC configurations, with workable lower-effects settings.
- [ ] Browser distribution and clean Windows offline install/update are tested.
- [ ] README describes actual delivered features; release notes and credits are complete.

## 9. Risk response

The largest uncertainties are Pokémon sprite sourcing/volume, the union of move and ability behavior across 240 species, content authoring throughput, and save/update reliability. Address them at P0–P5 rather than discovering them after all maps are built.

If production is slower than expected, adjust release timing, staffing, or proposed optional systems at review checkpoints. Preserve the eight-gym beginning-to-credits adventure as the identity of the project. Never silently replace a full-game goal with an endless prototype.

Expansion candidates after a stable release: breeding, more regional species, extra quest arcs, advanced challenge presets, and eventually online services if explicitly desired. Each requires its own design and engineering plan.
