# Pokémon Aster — game design proposal v0.1

Date: 2026-10-07. Repository: `imthegoat101/pokemon-2d`.

## 1. Intent and success criteria

The owner wants the best complete 2D Pokémon game we can realistically finish, playable on PC, with gameplay and interface designed together and plans saved to GitHub through the connected plugin. Confirmed choices: Pokémon fan game, original region, classic modernized progression, browser plus desktop delivery, and DS-inspired pixel art.

Success means a player can start a new game, explore a coherent region, catch and develop a team, earn eight badges, resolve the story, defeat the League, watch credits, and continue into meaningful postgame. All of this must work with keyboard alone and survive saving, closing, updating, and transferring between browser and desktop.

This document proposes the remaining creative and technical choices. It is a complete systems blueprint, not a claim that all maps, move records, dialogue lines, or art already exist. Content authoring and its validation gates are part of the plan.

## 2. Design pillars

1. **The Pokémon adventure comes first.** Catching, team-building, rival battles, gyms, and discovery remain central.
2. **A memorable place.** Each town has a purpose, each route has a visual identity, and revisits reveal new paths or stories.
3. **Strategic, understandable battles.** Difficulty comes from teams and decisions; important rules are visible in the UI.
4. **Respect the player's time.** Fast text, animation-speed options, free healing, accessible move reminders, and no forced grinding.
5. **Finish and polish.** A complete curated regional game takes priority over enormous species counts or unrelated systems.

## 3. Product scope

| Area | Proposed launch scope |
| --- | --- |
| Platforms | PC browser and Windows desktop; macOS/Linux packaging after Windows acceptance |
| Play model | Single-player; no account required; desktop fully offline |
| Campaign | 20–25-hour target, 8 gyms, 4 Elite Four members, Champion, story resolution |
| Postgame | 5–8-hour target, 3-area island arc, Battle Circuit, rematches, completion rewards |
| World | 11 settlements, 18 routes, 10 major exploration areas including postgame areas |
| Creatures | 240 unique obtainable species; evolution stages each count as a species |
| Quests | 32 authored side quests: 24 before credits, 8 after credits |
| Battles | Singles and selected doubles; 18-type system; physical/special split |
| Presentation | Top-down pixel world, animated front/back battle sprites, modern 16:9 menus |
| Language | English first; all strings externalized so Portuguese can be added later |
| Saving | 3 profiles, manual saves, autosave, rolling recovery snapshots, portable export/import |

No launch commitment to online trading, PvP, co-op, breeding, contests, open-world badge order, procedural maps, real-time combat, Mega Evolution, Z-Moves, Dynamax, Terastallization, or a thousand-species National Dex. These are expansion candidates only after launch criteria pass. NPC trades and deterministic quest evolution alternatives cover species that would otherwise require online trading.

## 4. The core loop

Explore a route → discover Pokémon and encounters → catch or battle → develop a team → discover a local problem → prepare for a gym → solve its challenge → win a badge and a traversal license → unlock the next area and new reasons to revisit earlier areas.

The short loop is a two-to-five-minute encounter, discovery, or small objective. The medium loop is a route, town, quest, and meaningful team adjustment. The long loop is a gym chapter and a story revelation. Avoid stretches of mandatory dialogue or identical trainer fights that crowd out exploration.

## 5. Player and overworld

- Choose name, pronouns, skin tone, and one of four outfit palettes; choices are cosmetic. No gender-locked progression.
- Four-direction tile-based movement with smooth sprite interpolation. Native tiles are 16×16 pixels; character art can extend beyond tile bounds. Collision uses authored tile/object properties.
- Walk, run, bicycle, surf, and licensed traversal interactions. Autorun toggle; bicycle unlocked after gym 2.
- Context interaction with NPCs, signs, doors, items, puzzles, and service counters. Display an interact cue only when a valid target is in front of the player.
- NPC facing, footsteps, dust, water ripples, foreground foliage, mild weather, and following lead Pokémon add life. Following sprites cover all 240 species and can be disabled; followers do not collide or trigger encounters.
- Trainer sightlines have clear visual spacing. No unavoidable battle immediately after a lengthy cutscene unless a recovery/save checkpoint precedes it.
- Indoor/outdoor transitions preserve facing and do not place the player on blocked tiles.
- A serviceable map records visited locations and unlocked travel nodes. Quest destinations are optional markers, not constant navigation arrows.

### Encounters

Use authored grass, cave, water, and fishing encounter tables. Random encounters remain the primary system; a few fixed visible Pokémon act as special encounters. There is no second full roaming-Pokémon simulation at launch.

After an encounter, provide eight eligible movement steps of immunity. Thereafter use a map-specific chance, initially tuned around one encounter per 18–28 eligible steps. Encounter RNG is independent from combat RNG. Repels suppress encounters below the lead Pokémon's level and offer reuse on expiry. Do not consume encounter steps during menu use or scripted movement.

Day/night uses an in-game clock, with a 48-minute cycle that advances only during active overworld play. Battles and menus pause it. Time-specific species always have a practical alternate availability window or postgame source. Resting at designated lodgings selects morning, afternoon, or evening without real-world waiting.

### Traversal

Badge rewards unlock field licenses used through context interactions: brush clearing, surveying clues, illuminated passages, ferry routes, rock moving, surfing, climbing, and aerial fast travel. These do not occupy battle move slots or require a specific party member. Ordinary shortcuts unlock from their far side. Never consume a unique key item to cross a mandatory gate.

## 6. Pokémon ownership and growth

Six party slots; 30 storage boxes of 30 slots each. A capture with a full party goes to storage. If storage is full, disable capture items before consumption and explain how to make space.

Each Pokémon stores a stable instance ID, species/form ID, nickname, level, experience, six IVs, six EVs, nature, ability, up to four moves and PP, held item, current HP/status, friendship, shiny flag, original trainer, and capture metadata.

Use standard level/experience growth groups, the standard six-stat model, level-up learning, and curated stone, friendship, and location evolution conditions. The roster may include later-generation Pokémon, but every chosen species must have fully supported evolution requirements. Trade evolution uses a reusable Link Charm unlocked by a side quest. Evolution triggers are explicit data; no hidden hard-coded exceptions scattered through UI.

Base stat proposal: IV 0–31; EV maximum 252 per stat and 510 total; standard nature adjustments. A Pokémon has one supported species ability, including optional hidden abilities on authored rare encounters. No IV or hidden-ability reroll loop is required to finish the campaign.

Evolution is prompted after battle rewards and move learning finish. Players can cancel, see why evolution was offered, and re-trigger eligible level evolutions on the next level. Stone evolution consumes the stone only after confirmation; inventory and creature update commit together. Early evolution does not permanently remove access to essential moves: a free reminder teaches eligible historical level-up moves at Pokémon Centers.

TMs are reusable. Held items, berries, status cures, evolution items, repels, and capture balls have distinct bag categories. Friendship requirements are reachable through ordinary adventure play. No breeding or egg simulation at launch.

### Experience and grinding

Classic mode: participating eligible Pokémon get full configured encounter EXP; eligible benched Pokémon get 50% with shared EXP enabled. Disabling shared EXP gives EXP only to participants, with no redistribution. Fainted Pokémon do not gain EXP. Trainer multiplier, species yield, level, and growth groups are defined in versioned data.

Badge caps apply to EXP-based leveling and Rare Candy use: 16, 24, 32, 40, 48, 56, 64, 70, then 100 after the Champion. At the active cap, EXP does not accumulate for a later release burst. UI states the cap clearly. Players can disable caps in Adventure mode. Enemy teams have authored levels; there is no invisible scaling to party level.

Postgame adds visible stat training, nature mints, and a limited reliable source of ability/IV improvement items. These create team-building goals without blocking the story.

## 7. Battle rules

The project uses a versioned **Aster Rules v1** profile: familiar modern Pokémon calculations with selected explicit project rules. It is not marketed as exact compliance with an entire official generation. Species stats, move values, learnsets, and ability behavior come from one documented baseline and cannot silently mix incompatible sources.

### Battle structure

- Wild and trainer singles; doubles in authored gyms, story encounters, and Battle Circuit matches.
- Turn choice: Fight, Pokémon, Bag, Run. Run works only in eligible wild battles; trainer captures are rejected before an item is spent.
- Four moves per Pokémon, with type, category, power, accuracy, PP, priority, target rules, and effects visible.
- Resolve switches, eligible items, and moves according to an explicit ordering contract. Switches and items precede ordinary moves. Move priority precedes effective speed; speed ties use seeded randomness. Trick Room reverses speed ordering within the same priority bracket.
- Target selection is required in doubles. A chosen target that becomes unavailable follows the move's authored retarget policy. Friendly fire moves preview eligible allied targets.
- End-of-turn effects have a fixed ordered registry. The battle engine emits semantic events; animations never determine damage, action order, or rewards.
- Player replacement after fainting does not grant an extra action. Trainer battles cannot escape. A total party faint ends the battle and invokes the overworld defeat flow.

### Mechanics contract

18 types including Fairy; immunities; physical/special/status categories; STAB; critical hits; random damage; stat stages from −6 to +6; abilities; held items; weather; terrain; hazards; screens; priority; protection; recoil; drain; multi-hit; multi-turn charging; switching; and secondary effects.

Direct damage uses the documented modern level/power/attack/defense structure, with a 16-value random damage factor from 0.85 to 1.00, 1.5× STAB, and 1.5× critical multiplier. Exact integer rounding and modifier order must be specified and covered by golden tests before the slice is accepted. Do not substitute a simplified formula while presenting it as finished Pokémon combat.

Burn, poison, bad poison, paralysis, sleep, and freeze are persistent conditions; confusion, flinch, taunt, encore, protect state, and other temporary effects are battle-only. Canonical details are resolved in the mechanics reference during foundation work, not improvised per move. All moves and abilities used by the slice are fully specified before that slice; all launch mechanics are finalized before full content production.

The content pipeline computes the union of required effect handlers from every obtainable species, learnset, trainer team, item, and evolution. An unsupported handler blocks that content from release. Descriptions must match actual behavior. If a mechanic is excluded, remove every route by which players could obtain it.

### Catching and fleeing

Wild capture uses a documented Aster capture formula based on species catch rate, HP, status, and ball modifier. Capture success is fixed by the engine before shake animation. Capture consumes one ball and the player's turn; a failed throw allows the opponent to act. Special ball conditions are displayed. No fake success percentages.

Legendary encounters are authored events, not random rolls on ordinary routes. Defeated or escaped story legendaries return through a documented post-story retry trigger. One-time gift Pokémon are marked claimed only after party/storage transfer succeeds.

Wild flee probability uses speed and attempt count with a documented cap. Guaranteed escape items and abilities are explicit handlers. A successful escape retains HP, PP, and status changes already applied.

### Rewards and defeat

Resolve EXP, levels, move learning, evolution, money, item rewards, Pokédex updates, and quest progress in an ordered results flow. Commit once; reloading cannot duplicate rewards. Trainer victory flags and rewards share the same transaction.

Classic defeat returns the player to the most recently registered healing point, heals the party, and deducts the lesser of 10% of carried money or a configured badge-scaled ceiling. Money never goes negative. Adventure mode has no money penalty. Quest/badge progress is retained. Battle Circuit defeats exit the run without campaign money loss.

## 8. Difficulty and AI

| Preset | Player experience |
| --- | --- |
| Adventure | Gentler teams; shared EXP and helpful effectiveness hints; no money loss; caps optional; Switch battle style |
| Classic — default | Authored teams and caps; shared EXP on by default; normal resource pressure; Switch/Set selectable |
| Expert | Stronger synergistic teams, Set style, no trainer-battle bag items, badge caps; same campaign and rewards |

Change difficulty at healing points outside battles and scripted events. Changing it does not discard progress. Moves, damage formulas, and catch rates do not secretly change between presets; trainer teams and documented restrictions do.

Wild AI uses weighted legal moves. Ordinary trainers prefer useful damage and avoid known immunities. Gym/story bosses use a limited score model that considers damage, publicly visible statuses, matchup, setup, switching costs, and team roles. Decisions use only opponent information legitimately revealed in battle, plus their own team. They never read the player's chosen action or unrevealed moves.

Bosses get a finite configured consumable budget visible through use; no unlimited healing. Expert trainers follow the same no-bag-item rule as the player. Trainer classes foreshadow their strategy, and pre-gym NPCs teach the relevant mechanic.

## 9. Region, story, and progression

**Asteria** is a coastal region connected by orchards, wetlands, old observatories, wind farms, mountain paths, and offshore islands. Its identity is the contrast between a living ecosystem and a culture studying the stars. The region uses Pokémon already known to players; original lore belongs to its people and places.

The player's League challenge runs alongside a research assignment for Professor Laurel. Unusual migrations and navigation failures lead to **Team Meridian**, a civic energy organization trying to synchronize the region's power grid using a legendary Pokémon. Its public work is useful; its forced control of habitats is the conflict. Keep dialogue concise and let changed maps, displaced Pokémon, and NPC situations tell part of the story.

A confident rival, Rowan, values battle achievement. A research-minded friend, Mira, values discovery. Both grow through the journey; neither interrupts every route with exposition. Gym leaders have civic roles and participate in their local chapter.

Three acts: learn the League journey and notice ecological anomalies; investigate Meridian while exploring increasingly complex habitats; interrupt its forced synchronization at the observatory, then finish the final gym and League on the player's own merits. Story victory does not award the Champion title automatically.

The full town/gym table, unlocks, chapter beats, and content budgets are in [CONTENT_PLAN.md](../../CONTENT_PLAN.md).

Gym puzzles are short introductions to their area's mechanic. Completed puzzle routes remain solved. Gyms 1–2 teach fundamentals, 3–4 weather and status, 5–6 doubles and switching, 7–8 advanced synergy. Each gym has a recovery point or practical return shortcut.

The League consists of four sequential battles and a Champion. A lobby allows final preparation, shopping, saving, and storage. First entry clearly explains that between-match bag healing is allowed, storage is unavailable during the run, and defeat restarts the run. Players may save between battles; no softlock on resuming a League save.

## 10. Quests, exploration, and services

Journal entries contain objective text, actual progress, location hints, and rewards. Track one quest at a time. Mandatory objectives have explicit stage flags. Optional quests never consume unique progression items or permanently lock campaign routes.

Quest variety includes NPC stories, rescue/exploration, ecology observations, team-building, puzzles, and optional challenges. Avoid requirements for extremely rare random drops. Ownership-based quests accept Pokémon caught before the quest. No mission requires releasing a unique Pokémon.

Pokémon Centers offer free full healing, storage, move reminder, nickname changes, and stat explanations. Shops offer badge-tier inventory with previews of price, owned quantity, and effect. Purchase quantity is limited by money and inventory capacity before confirmation. NPC trades show offered/requested species and preserve a documented ownership history.

Start with 3,000 money, five Poké Balls, five Potions, and a free heal tutorial. Proposed staple prices: Poké Ball 200, Potion 300, Antidote 100, Repel 350, Super Potion 700, Great Ball 600, Ultra Ball 1,200. Trainer payout scales by class and maximum team level, with a chapter review ensuring enough income for reasonable catch and heal play. Rare items and held items are awarded through exploration and quests, not required expensive purchases.

No survival meters, crafting economy, daily login rewards, cash shop, or real-world timers. Shops, fishing, trainer rematches, hidden paths, and Battle Circuit rewards are sufficient repeatable activities.

## 11. Postgame

- Three island exploration areas complete the Meridian aftermath and provide legendary retries.
- A five-match Battle Circuit run normalizes battle level to 50 without altering owned levels, restores the party between matches, prohibits bag items, and gives fixed run rewards. Player party and opponents use the same rules.
- Weekly-sounding events are avoided: rematches are unlocked by completed objectives or Circuit runs, not by waiting on the real calendar.
- Gym leader rematches and an enhanced League round use six-member teams.
- Obtain every regional species in one save through wild encounters, gifts, NPC trades, quest rewards, or explicit evolution alternatives. No version exclusives or online dependency.
- Pokédex completion rewards cosmetics, a title, and a shiny-rate improvement; exact improved odds belong in the versioned rules. Normal shiny odds proposed at 1/4,096.
- Eight postgame side quests resolve character arcs and support team optimization.

## 12. Interface, controls, and presentation

The entire game must be keyboard-playable. Mouse accelerates menu use; controller support is included after the slice. Focus remains visible, shortcuts are contextual, and confirming a menu cannot also move the player or dismiss the next dialog.

World art uses a pixel-snapped camera and nearest-neighbor scaling. Proposed world viewport: 480×270 logical pixels. UI renders at display resolution over that world so text stays readable at 720p and 1080p. Menus do not imitate the DS dual-screen hardware; they use the PC viewport intelligently.

Warm ivory panels, deep navy text, teal navigation accents, and restrained amber highlights give the game a recognizable identity. Environment palettes vary by biome. Pokémon type colors supplement icons and labels. Animation priority: idle life, walking, key attack families, fainting/capture/evolution, and transitions that never obscure input readiness.

Every screen, dialog, layout region, interaction state, and shortcut is specified in [UI_UX.md](../../UI_UX.md). Art and audio production requirements are in [CONTENT_PLAN.md](../../CONTENT_PLAN.md).

## 13. Saves, reliability, and accessibility

Three independent profiles. Each stores manual save, latest safe autosave, and two prior recovery snapshots. Manual saving is available in idle overworld and between League matches. Saving mid-animation, mid-turn, mid-transfer, or mid-script is unavailable in v1.

Autosave follows completed map transitions, heals, captures, quest rewards, badges, and resolved battles. If a write fails, show a persistent unsaved indicator and export option; never claim success. Export/import works across browser and desktop for compatible schema versions.

Browser saves are local to that browser/site and can be cleared. Explain this on first save and offer a backup button. Cloud sync is not promised. Desktop saves use an application data folder through a storage adapter. Recovery preserves the last validated snapshot. Import validates the whole file before modifying a profile.

Full remapping, hold/toggle running, three text speeds plus instant text, animation speed, reduced motion, flash reduction, independent audio buses, visible effect labels, selectable UI scale, and confirmation for destructive actions are launch features. No puzzle requires rapid button presses or color recognition alone. Screen-reader narration of map navigation is not promised in v1; semantic menus and structured battle summaries should remain compatible with future accessibility work.

## 14. Technical direction and boundaries

Recommended: TypeScript, a verified pinned Phaser version, Vite, authored Tiled maps, schema-validated content, and Tauri for the desktop package. Browser and desktop share game systems and save formats. Pin actual versions only after a compatibility probe; current documentation includes multiple Phaser majors, so copy examples from the chosen major exclusively.

Keep battle simulation independent from Phaser, drawing, audio, UI, and persistence. World scripts, save services, content loaders, input routing, and battle presentation have narrow contracts. The core can replay a seeded battle headlessly. Pokémon data is bundled with the game; no runtime API outage can prevent play.

The architecture, content contracts, source references, save protocol, and meaningful verification strategy are in [TECHNICAL_DESIGN.md](../../TECHNICAL_DESIGN.md).

## 15. Quality targets and completion definition

Target 60 FPS during ordinary exploration on a measured integrated-GPU PC at 1080p with a reduced-effects fallback. Hardware and browser versions must be recorded during the slice; this is a target, not an untested minimum-spec claim. Aim for a compressed first-play payload under 25 MB, with later area packs loaded on demand. Desktop ships all packs locally.

Complete means all 240 species are obtainable; eight gyms, story, League, credits, and postgame are playable; all referenced effects and assets resolve; saves survive migration and interruption; no known progression softlocks or critical bugs remain; controls and UI pass keyboard-only review; and a clean machine can launch the browser and installed desktop builds.

Content targets cannot be called complete because counters or placeholder menus exist. A gym is complete only with its map, puzzle, teams, rewards, dialogue, retry path, and tested progress flags. Every major system has similar acceptance gates in [PRODUCTION_ROADMAP.md](../../PRODUCTION_ROADMAP.md).

## 16. Review checkpoints

Review the working identity, 240-species/20–25-hour content budget, mechanics profile, story concept, and technical direction before implementation. Then derive detailed plans in this order: platform/save contracts; world and input; battle core; first-gym content; campaign production; postgame; release hardening.

The first-gym slice supplies measured evidence about art workload, battle effect coverage, authoring speed, and cross-platform behavior. Adjust production volume or staffing from that evidence while retaining a complete beginning-to-credits adventure. No calendar promise is made before that checkpoint.
