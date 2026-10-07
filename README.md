# Pokémon Aster — project plan

A complete 2D Pokémon fan adventure for PC, set in the original Asteria region.

**Status:** design proposal v0.1, ready for owner review. This repository currently contains planning documents, not a playable game. Pokémon Aster and Asteria are working names.

## Confirmed direction

- Actual Pokémon in an original region and story.
- A classic, modernized journey: eight gyms, Pokémon League, side quests, and postgame.
- PC browser play and a downloadable desktop build.
- DS-inspired pixel art with modern, readable menus.

## Read the plan

| Document | Purpose |
| --- | --- |
| [Game design](docs/superpowers/specs/2026-10-07-pokemon-aster-design.md) | Vision, gameplay rules, progression, story, difficulty, and release scope |
| [UI and controls](docs/UI_UX.md) | Every major screen, navigation, layouts, input, accessibility, and UI states |
| [Region and content](docs/CONTENT_PLAN.md) | Towns, gyms, chapter progression, quests, Pokémon roster strategy, art, and audio budgets |
| [Technical design](docs/TECHNICAL_DESIGN.md) | Browser and desktop architecture, data contracts, saving, tools, and validation |
| [Production roadmap](docs/PRODUCTION_ROADMAP.md) | Dependency order, deliverables, playable acceptance gates, and release definition |

## Proposed release

Single-player, offline-capable desktop game; browser game with local saves and portable save-file export. Proposed content target: 240 obtainable species, eight gyms, 11 settlements, 18 connecting routes, 10 major exploration areas, 32 side quests, and a postgame tournament and legendary quest arc. Campaign duration target: 20–25 hours, subject to playtesting.

The first development milestone is a polished route-to-first-gym slice, including catching, party management, a complete gym, saving, and browser/desktop smoke tests. This is the foundation for the full game, not a reduction of the final scope.

## Decisions to review

The owner chose the four directions above. The working title, exact species roster, story, scope budgets, offline single-player model, mechanics profile, and Phaser/TypeScript plus Tauri architecture are proposed defaults. They can be revised before product implementation begins.

The roadmap is a release-level production plan. Detailed implementation tasks should be derived from the reviewed design, subsystem by subsystem. No release date is promised until the vertical slice establishes actual production throughput.

## Asset policy

Do not commit ripped ROMs, game music, credentials, or assets with unknown provenance. Record source and usage terms for each shipped asset. Project-created region art, interface assets, dialogue, and music are the default production path; Pokémon asset sourcing is an explicit unresolved production dependency, with an actionable decision gate in the roadmap.

This is an unofficial fan project. This repository does not grant rights to Pokémon names, characters, artwork, or music. Distribution and monetization are separate decisions from this game design.
