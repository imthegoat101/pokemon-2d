# Pokémon Aster — technical design

Status: proposed architecture v0.1. Product implementation has not begun. This document defines boundaries and contracts; detailed implementation tasks follow design review.

## 1. Platform choice

Recommend TypeScript + Phaser + Vite for the browser game, authored Tiled JSON maps, schema-validated bundled content, and Tauri for a Windows desktop package. Both targets run the same battle logic, maps, UI, and content. Platform adapters handle saving, export/import dialogs, fullscreen, and lifecycle events.

Phaser provides scenes, input, cameras, sprites, and tilemaps. Tauri hosts the frontend and offers controlled native integration. These capabilities fit the chosen browser-first product; the recommendation is an architectural judgment rather than proof that our eventual game meets performance targets.

Alternatives evaluated: Godot has strong editor workflows and native export, but its web export has documented browser/renderer constraints; an RPG Maker/Pokémon Essentials route could speed familiar desktop systems but would require a separate compatibility investigation for the browser-first requirement. Choose one runtime, not two parallel game implementations.

Before scaffolding, verify current stable compatible package versions and pin a single Phaser major. Its documentation includes multiple majors; scene APIs and plugins must match the selected version. Probe tilemaps, overlay UI, audio unlock, keyboard/gamepad input, desktop offline loading, and save writes. Do not choose a version number from memory.

Primary references, consulted 2026-10-07:

- [Phaser scenes](https://docs.phaser.io/phaser/concepts/scenes)
- [Phaser input](https://docs.phaser.io/phaser/concepts/input)
- [Phaser tilemap reference](https://docs.phaser.io/api-documentation/class/tilemaps-tile)
- [Tauri frontend configuration](https://v2.tauri.app/start/frontend/)
- [Tauri capabilities](https://tauri.app/reference/acl/capability/)
- [Godot web export limitations](https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_web.html)

## 2. Modules and responsibilities

| Module | Owns | Excludes |
| --- | --- | --- |
| Battle core | Legal actions, turn order, RNG, effects, damage, fainting, victory | Rendering, timers, persistence, DOM |
| Creature domain | Stats, ownership, learning, evolution, party validity | Sprites, menu focus |
| World runtime | Tile movement, collision, warps, NPC interactions | Battle damage calculations |
| Script engine | Dialog, conditions, commands, checkpoints | Arbitrary evaluated scripts from content |
| Quest service | Objectives, subscriptions, stage/reward validation | Button rendering |
| Content registry | Immutable validated species/move/map/item definitions | Mutable player state |
| Campaign state | Flags, money, badges, inventory, party, location | Renderer references or scene instances |
| UI layer | Panels, navigation, descriptions, focus, accessibility labels | Direct mutations of campaign data |
| Battle presenter | Consume semantic events into sprites, text, sound | Alter outcomes based on animation timing |
| Save service | Snapshots, validation, migrations, transaction outcomes | Gameplay decisions |
| Platform adapter | Browser/desktop storage, dialogs, lifecycle | Platform-specific battle rules |
| Asset/audio service | Load packs, atlas references, sound buses | Quest flags and capture chances |

Use Phaser for world and battle presentation. Use a semantic HTML overlay for menus, settings, lists, forms, and text; CSS follows the game's visual system. Avoid adding a separate component framework unless UI complexity demonstrates a need. The overlay and renderer share a viewport transform and one input router; they do not each listen independently for confirm.

Proposed source areas: `core/battle`, `core/creatures`, `core/campaign`, `world`, `scripts`, `content`, `ui`, `presentation`, `services`, `platform/browser`, `platform/desktop`, and `tools`. Tests sit by meaningful domain behavior or in focused integration suites. These are module boundaries, not a requirement for a large file count before the slice.

## 3. Battle contract

Input: validated immutable encounter definition, owned Pokémon snapshots, rule profile, and explicit seeded RNG state. Commands: select move/target, switch, use item/target, attempt flee, and required replacement. Output: updated battle state and ordered semantic events.

Events include action used, hit/miss, HP changed, status/stage changed, field effect changed, PP changed, item consumed, switched, fainted, caught, escaped, and battle ended. Each has stable IDs and sufficient information for presentation without reading private opponent data.

The engine rejects illegal commands before item consumption or turn advancement. Missing effects are startup/content-validation errors, never no-op moves. A deterministic run with the same initial state, seed, and commands must yield the same final state and events. Battle RNG is saved in safe persisted encounter results; overworld randomness and visual particles use separate streams.

Post-battle rewards belong to the campaign transaction service. The battle engine reports outcomes and earned contributions; it cannot directly write a save or mark a gym cleared. Rewards, trainer victory, quest events, captures, and progress flags commit together to avoid duplicate money or lost catches.

## 4. Content contracts

| Record | Required fields / invariants |
| --- | --- |
| Species | ID, national metadata, regional number, types, base stats, growth, abilities, learnset, evolution, catch rate, assets |
| Move | ID, type, category, power, accuracy, PP, priority, target policy, effect IDs and parameters |
| Ability | ID, description key, registered trigger handlers and parameters |
| Item | ID, category, price/sell rules, stack limit, valid contexts, effect handler, assets |
| Trainer | ID, preset teams, class, payout, text keys, encounter/victory/rematch flags |
| Encounter table | Map/terrain/time conditions, weighted species/form/level ranges, availability prerequisites |
| Map | ID, tile layers, collision, objects, spawn points, warps, encounters, scripts, pack/music IDs |
| Quest | ID, prerequisites, ordered objectives, event filters, rewards, NPC/dialogue links |
| Evolution | Source/target, condition, consumed items, alternate method where needed |
| Asset | ID, file, dimensions/frames, source/terms, palette/form, attribution |
| Rules profile | Version, formula order, status durations, caps, EXP, catch/flee, preset restrictions |

Validate all cross-references, probabilities/weights, dimensions, legal teams, evolution connectivity, inventory limits, and handler coverage at build time. Generate reports for obtainable-species coverage, chapter availability, required move/ability handlers, missing sprites, and asset provenance.

Stable string IDs remain unchanged across saves even if display names or regional ordering change. Content packs have version IDs and manifest hashes. No runtime dependence on PokéAPI or another public data service; imports, if used, are development tooling with provenance and review. Do not scrape arbitrary asset sites during gameplay.

## 5. World and scripting

World movement operates on tile cells; interpolation is presentation. Interactions occur at completed steps or explicit action boundaries. Scene transitions resolve destination spawn/collision before applying player position. A follower is presentation state derived from the lead instance, not a second authoritative movement actor.

Scripts use a finite command vocabulary: show dialogue, offer choice, move actor, start battle, set/check flag, grant/take item, grant Pokémon, update quest, play cue, unlock traversal, and warp. Each command validates preconditions and applies through domain services. Scripts have an execution budget and no unrestricted code evaluation.

Persistent checkpoints occur only at declared safe boundaries. During a non-resumable script, a reload returns to its preceding safe checkpoint. Reward commands have unique transaction IDs, so replaying a scene cannot grant the same reward again. Mandatory gifts first ensure party/storage capacity or provide a safe storage-access route.

## 6. Save format and failure protocol

Envelope: schema version, rules version, content compatibility version, profile ID, snapshot ID, creation/save timestamps, payload checksum, and campaign payload. Payload includes player, location/facing, in-game clock, party/storage, inventory, money, badges/licenses, quests/flags, encountered/owned Dex state, RNG streams, play time, and profile-specific settings. Do not serialize DOM, Phaser objects, timers, or arbitrary functions.

Three profiles each retain manual snapshot, latest autosave, and two older recovery snapshots. A manual save identifies an intentional checkpoint; autosaves do not overwrite that slot. Each successful write creates an immutable snapshot and changes profile metadata to point at it.

Browser: IndexedDB with a transaction updating snapshot and pointers together. Desktop: an application-data save directory with write-to-temporary, validate, atomic-replace where supported, and retained prior snapshot. A checksum detects accidental corruption, not cheating or malicious changes. Only expose the native file/dialog permissions required by this adapter.

Sequence: request at a safe boundary → freeze/copy serializable state → schema validate → write → reread/verify where supported → update visible successful-save state. Coalesce repeated autosave requests and serialize writes. On failure, retain prior snapshots and show unsaved state with retry/export.

Capture/reward commands commit to in-memory campaign state atomically and request a safe autosave. If persistence fails, the game keeps the earned result in memory and reports it as unsaved; never undo a capture silently. Reloading necessarily restores the last successful snapshot, which the UI explains honestly.

Import: parse with file-size bounds, check schema and IDs, migrate on a copy, validate party/storage/progression invariants, preview, confirm replacement, preserve current recovery copy, then commit. Unsupported future schemas are rejected without changes. Export contains a plain portable envelope; browser and desktop imports have cross-target fixture tests.

Migration: one pure conversion per schema step, old-save fixtures, original backup preserved, explicit handling of removed/renamed content IDs. No automatic reset on migration failure. Safe manual/auto snapshots between League matches include remaining opponent index and run state.

## 7. Loading and desktop behavior

First pack includes title, settings, player, starter area, first-route encounters, initial battle assets, and minimum audio. Load neighboring area packs opportunistically; loading state offers retry if needed. Release acceptance measures the proposed under-25-MB compressed first-play target. Pack sizes and creature animation budgets must be measured, not guessed.

Browser first visit requires connectivity. Cached areas can be available afterward, but full offline browser play is not a launch guarantee; desktop contains every required pack and works without networking. Deploy updated pack manifests atomically so a session cannot mix old rules with new assets. Display an update prompt after a safe save, never replace data during a battle.

Desktop gates: offline asset load, app-data writes, file dialogs, gamepad, fullscreen, resize/DPI changes, audio, clean install/update, and uninstall behavior that does not silently remove saves. Validate Windows first; other operating systems are separate packaging checks. A wrapper compiling successfully is not evidence that the game works offline.

## 8. Verification strategy

| Risk | Meaningful verification |
| --- | --- |
| Combat correctness | Golden calculations, priority/speed ties, immunities, status timing, doubles targets, seeded replay |
| Illegal item/turn use | Trainer capture rejected, unavailable target/item rejected without consumption |
| Content gaps | All references and effects valid; every roster species has an attainable source |
| Reward duplication | Retry/load at boundaries cannot duplicate money, Pokémon, badges, or quest gifts |
| Save loss | Interrupted write, quota failure, corrupt snapshot recovery, migration, cross-platform import |
| Progress softlocks | Mandatory world-gate graph plus end-to-end runs through every chapter |
| UI/input failures | Keyboard-only critical paths, modal input isolation, gamepad disconnect, 720p/1080p layouts |
| Distribution | Clean-browser and clean-Windows install, offline desktop start, upgrade preserving saves |

Use a focused unit test runner for pure rules, integration tests for campaign transactions/save services, and browser automation for UI journeys. Choose specific tools during implementation planning; do not write tests that only repeat an object definition or screenshot placeholders.

Manual playtests remain necessary for pacing, battle fairness, confusing quests, navigation, and art coherence. Record findings with reproducible save/seed/location where possible. Developer diagnostics include current map/tile, flags, pack versions, seed, frame timings, and last script command; they are disabled in normal play.

## 9. Release operations

GitHub holds design, code, authored data, and asset manifests. CI later runs type checks, content validation, rules tests, relevant UI integration, browser build, and desktop build checks. Do not add CI workflows until the corresponding commands exist.

Release tags identify game/rules/content/save versions and include checksums, changelog, known limitations, desktop installer, and browser deployment link. Hosting selection is a future operational decision; browser delivery should remain static and should not require a paid game backend. No analytics or crash-data upload is enabled without an explicit product decision.
