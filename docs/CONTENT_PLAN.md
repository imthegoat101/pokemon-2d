# Pokémon Aster — region, campaign, and content production

Status: proposed v0.1. Names, teams, and story direction are editable defaults. Counts below are production budgets, not completed content records.

## 1. Region structure

11 settlements: one starting village, eight gym towns, one League settlement, and one postgame island settlement. Eighteen numbered routes connect the campaign; optional interior connections are part of their containing maps. Ten major exploration areas include caves, wetlands, ruins, facilities, and three postgame areas. Buildings and puzzle rooms are additional maps, not additional settlements.

Progression is linear by badge with local optional loops. Hubs connect to older areas through shortcuts; traversal licenses open useful optional paths. Players can backtrack freely except during short explicitly bounded story sequences.

| Chapter / place | Identity and routes | Major beat | Gym / leader | Ace and cap | Unlock |
| --- | --- | --- | --- | --- | --- |
| Prologue — Bellroot Village | Orchards; R01 | Professor Laurel's research; starter and rival introduction | None | Starter level 5 | Catching, journal, map |
| 1 — Verdant Town | Greenhouse community; R02 | Repair irrigation and notice displaced wild Pokémon | Grass — Iris; irrigation puzzle | Roselia Lv16; cap16 | Brush clearing |
| 2 — Quarrygate | Stonework town; R03–04 | Meridian funds a quarry expansion; discover habitat damage | Rock — Calder; minecart routing | Lycanroc Lv24; cap24 | Survey license, bicycle |
| 3 — Brineport | Fishing port; R05–06 | Ferry navigation anomalies; Mira investigates | Water — Marin; water-level puzzle | Floatzel Lv32; cap32 | Lantern license |
| 4 — Zephyr City | Wind farm and transit; R07–08 | Recover research data from a disrupted substation | Electric — Ada; circuit connections | Ampharos Lv40; cap40 | Ferry access |
| 5 — Emberfall | Hot springs and old foundry; R09–10 | Rowan loses a major battle; Meridian's plans become public | Fire — Sol; safe steam-valve puzzle, selected doubles | Arcanine Lv48; cap48 | Rock moving |
| 6 — Marshveil | Wetlands and conservation; R11–12 | Rescue a monitoring team and expose coercive capture | Poison — Neris; antidote/sluice logic | Toxicroak Lv56; cap56 | Surf license |
| 7 — Frostline | Alpine settlement; R13–14 | Secure the observatory approach and challenge Meridian | Ice — Eira; sliding rooms with reset points | Froslass Lv64; cap64 | Climbing license |
| Story climax | Observatory complex via R15–16 | Stop forced grid synchronization and restore migration | Meridian director plus optional legendary encounter | Story boss up to Lv66 | Safe access to final city |
| 8 — Astral City | Observatory culture and night markets; R17 | Resolve rivals' arcs and prove League readiness | Psychic — Orion; constellation perspective puzzle | Gardevoir Lv70; cap70 | Aerial travel |
| Finale — Crown Harbor | League campus; R18 / Victory passage | Elite Four, Champion Selene, Hall of Fame and credits | Four specialists and Champion | Lv68–70 first League | Cap100; postgame |
| Postgame — Haven Isle | Island research base and three exploration areas | Meridian aftermath, legendary retry quests, Circuit | Rematches and Circuit | Authored Lv70–85 or normalized Lv50 | Completion and optimization rewards |

Leader teams emphasize strategy rather than only one type. Grass teaches status/support; Rock teaches resistances; Water rain; Electric speed control; Fire doubles; Poison disruption/switching; Ice weather/coverage; Psychic setup and counterplay. The ace is proposed; final teams must be mechanically and thematically valid under the locked roster.

Gym teams by preset grow from 2–3 members early to 4–6 late. Expert gyms use four members in gym1, then five or six as the roster opens. Early answers to every gym strategy must be available before its battle. Avoid requiring the player to replace their starter to proceed.

## 2. Story beats and characters

**Professor Laurel:** studies navigation and habitat patterns; gives a concrete research task instead of a quest to save the entire world at the start.

**Rowan:** confident rival who believes rank measures growth. Battles at prologue, chapters 2, 4, 6, and pre-League. Chooses the starter with a type advantage over the player's choice. Their development culminates in a respectful final preparation battle.

**Mira:** research friend who explores habitat clues and supports optional field quests. Has a distinct team and participates in one scripted double battle; temporary allied Pokémon never become permanent player-owned instances.

**Director Voss / Team Meridian:** an infrastructure leader convinced that synchronizing a legendary's power will end regional shortages. Early maps show tangible benefits of their work; later quests reveal the cost. Their story ends with accountability and repair work rather than disappearing after a battle.

**Champion Selene:** appears twice during the campaign as a capable local participant. Their balanced team is foreshadowed through public exhibition information, not spoiled in an introductory menu.

Dialogue targets: short exchanges of 2–6 boxes for ordinary story scenes; longer scenes separated by player-controlled exploration. Each chapter needs an opening trigger, investigation task, local resolution, gym clear, and return-to-route transition. Optional dialogue updates after major events.

### Main quest state sequence

`starter_received → catch_training_complete → badge_1 → quarry_evidence → badge_2 → harbor_report → badge_3 → substation_recovered → badge_4 → foundry_exposed → badge_5 → wetlands_rescue → badge_6 → mountain_access → badge_7 → meridian_resolved → badge_8 → league_won → postgame_started`.

These are named milestones, not every script command. Mandatory triggers must be idempotent. Skipping or condensing a tutorial cannot skip items or flags. Some optional investigations can complete early; journals reconcile that progress on acceptance.

## 3. Exploration area budget

| Area | First access | Purpose and signature |
| --- | --- | --- |
| Bellroot Grove | Chapter1 | Catching, hidden clearing, starter-friendly optional battles |
| Echo Quarry | Chapter2 | Environmental clue, minecart paths, Rock/Ground availability |
| Brine Tunnels | Chapter3 | Lantern paths, tidal objects, optional fishing rewards |
| Zephyr Substation | Chapter4 | Meridian evidence, circuit routes, Electric encounters |
| Emberworks | Chapter5 | Steam puzzle, doubles encounter, optional held item |
| Mirror Wetlands | Chapter6 | Rescue sequence, surf revisit, Poison/Water biodiversity |
| Astral Observatory | Chapters7–8 | Story climax, star-map rooms, legendary retry anchor |
| Haven Reef | Postgame | Offshore exploration, rare water lines, quest branch |
| Haven Ruins | Postgame | Legendary clue puzzle, Ghost/Psychic lines |
| Haven Crater | Postgame | Final aftermath encounter and rare late-game species |

Victory passage is part of R18's content package, so it does not add an eleventh major area. Individual area packages can contain multiple connected maps. Reset levers and exits prevent puzzle softlocks; every traversal-gated return path has an authored alternative.

## 4. Pokémon roster strategy

240 species means about 90–105 evolution families, with final family count set when the explicit roster is authored. Choose by ecology and team role rather than inclusion of every popular species. Proposed starter trio: Treecko, Chimchar, and Piplup, with all nine stages in the regional roster. Other two starters become obtainable through postgame research gifts, ensuring one-save completion.

| Availability tranche | Newly obtainable species budget | Cumulative |
| --- | --- | --- |
| Prologue / gym1 | 24 | 24 |
| Gyms2–3 | 54 | 78 |
| Gyms4–5 | 60 | 138 |
| Gyms6–7 | 54 | 192 |
| Gym8 / League | 24 | 216 |
| Postgame | 24 | 240 |

Evolution-stage availability is counted when the stage can actually be obtained, not merely when its unevolved family appears. Gifts, stones, friendship, and level caps affect that timing. These counts are authoring targets, not automatic generation rules.

Early family candidates: Starly, Shinx, Bidoof, Budew/Roselia/Roserade, Caterpie, Wooper, Geodude, Ralts, Zubat, Mareep, and Rockruff lines. Select actual forms carefully: every alternate form requires its own supported stats, evolution, move compatibility, sprites, and encounters. Do not imply a form exists because the base species does.

First-gym slice locks exactly 24 supported species, including all three starter base stages, the species available on Bellroot routes/grove, and trainer-only early evolved stages. Those trainer-only species do not count as obtainable until their acquisition conditions are reachable. A slice-specific roster table must distinguish supported from obtainable records so the production totals remain honest.

Roster acceptance:

- All 18 types appear, with multiple useful roles; early routes provide answers to upcoming gyms.
- No species depends on unsupported battle effects, online evolution, breeding, unavailable items, or impossible time conditions.
- Every species has an acquisition path and all evolution prerequisites reachable in one save.
- Every family has a reason to belong to its habitat and chapter.
- Legendary allocation is at most six species within the 240 total. The story anchor proposal is Jirachi, with obtainable access or retry after the climax; other legendary selections require narrative and mechanics review.
- Every learnset references implemented moves, and every ability and held item has functioning handlers.
- Regional Dex numbering follows families and discovery pacing; official national IDs remain metadata.

The exact 240-name manifest, encounter weights, trainer parties, and learnsets are content-authoring deliverables before the campaign expansion gate. They are not filled with arbitrary species just to satisfy a count in a design draft.

## 5. Quest allocation and examples

32 unique side quests: 8 NPC stories, 8 ecological tasks, 6 exploration/puzzles, 6 team-building lessons, and 4 optional battle challenges. Allocate 3 pre-credits quests to each of the eight gym chapters, for 24 total; 8 postgame quests finish the allocation.

| Example quest | Objective | Acceptance/reward design |
| --- | --- | --- |
| Orchard Trouble | Find a broken irrigation part and clear a short grove path | Progress from actual world interaction; Potion bundle and local shortcut |
| A Place for Budew | Register Budew and speak to the greenhouse keeper | Accept prior ownership; berry reward and friendship explanation |
| First Team Lesson | Win an optional practice battle using a resisted matchup explanation | Tutorial reward does not require a particular starter |
| Quarry Echoes | Collect three visible survey notes | Never random drops; Link Charm quest chain |
| Lost Harbor Lantern | Explore Brine Tunnels and return a key object | Registered lantern cosmetic and fishing access |
| The Quiet Turbine | Solve a three-node circuit and report habitat observations | Reusable TM appropriate to this chapter |
| Rowan's Detour | Optional character conversation and trainer challenge | Rival development and held item |
| Wetlands Watch | Register three habitat species | Prior catches count; useful status-cure stock |
| Observatory Letters | Find records on revisited routes | Lore resolution and rare evolution item |
| Haven Restoration | Finish three local recovery tasks after credits | Legendary retry access; progress stays persistent |

Each quest record includes NPC/location, prerequisites, ordered objectives, event subscriptions, reward command, return destination, and post-completion dialogue. Quest progress cannot increment twice for one battle/capture event. Reward collection is idempotent and indicates storage failure if a gift cannot be transferred.

## 6. Trainer and item production budgets

Target 160 placed battle encounters: 104 ordinary route/area trainers, 24 gym trainers (3 per gym), 8 leaders, 8 rival/story character encounters, 8 Meridian encounters, 5 League opponents, and 3 unique optional/postgame bosses. Rematches and Circuit opponent templates are additional datasets, not new placed NPC counts.

Prepare 24 Circuit team templates split across introductory, middle, and advanced sets. Author rematch teams for all eight leaders and five League opponents. Each trainer has a stable encounter ID, preset-specific team, payout parameters, pre/post-battle text, victory flag, and rematch rule.

Initial item catalog budget: roughly 90 records across balls, medicine, repels, berries, evolution items, held items, TMs, and key objects. Exact count follows mechanics coverage. Every placement must refer to an existing item and quantity, with one-time pickup flags. Key progression rewards have guaranteed acquisition paths.

Move count follows supported learnsets, not an arbitrary quota. Expect several hundred move records for 240 species; measure the handler union before committing the final roster. Shared effects such as damage, status, boosts, weather, and recoil reduce implementation work, but each move still needs accurate data and meaningful verification.

## 7. World art and creature asset pipeline

| Asset group | Budget / specification |
| --- | --- |
| Environment tilesets | 7 families: town/interior, orchard/forest, quarry/cave, coast/wetlands, industrial, alpine, observatory/ruins |
| Player | 4 cosmetic palettes; 4 directions with walk/run; bicycle/surf states |
| NPC archetypes | 24 overworld archetypes plus named-character variants |
| Pokémon battle sprites | 240 front and 240 back sets; shiny palette variants; idle and hit/faint presentation |
| Pokémon followers | 240 four-direction overworld sets, with size categories |
| Named portraits | Professor, 2 friends/rivals, director, 8 leaders, 4 Elite Four, Champion: 18 people |
| Battle backgrounds | 10 biome/service variants with day/night palettes |
| VFX | About 30 reusable effect families, mapped to moves with type/category variation |
| Interface | Buttons, frames, 18 type icons, status icons, 8 badges, bag items, map nodes |

Do not treat "animated sprites" as a source-free asset. The large front/back/follower requirement is a real production dependency. Prototype with clearly marked original placeholders only in development; no placeholder Pokémon art in a release declared complete.

Asset manifest fields: stable asset ID, local path, creator/source URL, usage terms, attribution, dimensions, frame metadata, palette/form linkage, and production status. Source-control map definitions and original art source files when practical; distribute optimized runtime assets. Record provenance before production-scale asset acquisition.

One sprite style guide fixes light direction, outline weight, palette behavior, silhouette scale, ground anchor, and animation cadence. Review a starter, small bird, large quadruped, and floating Pokémon together before authoring the full set. Nearest-neighbor rendering alone does not make mixed assets coherent.

## 8. Audio direction

Original or appropriately licensed looping music. Proposed 18 tracks: title; hometown; cheerful town; coastal town; industrial town; mountain town; overworld day; overworld night; cave/ruin; Meridian area; wild battle; trainer battle; gym battle; rival battle; Meridian boss; League/Champion; postgame/Circuit; credits. Reuse with location-specific instrumentation rather than commissioning a track for every room.

Five short jingles: healing, capture, level-up, badge, and quest completion. About 50 reusable UI/world/battle sounds plus creature cries if usable source material is available. Cries use an explicit manifest; silently missing cries are not a finished feature. Provide separate master/music/ambience/SFX buses, duck music during important cues, and avoid overlapping the same jingle repeatedly during rewards.

Music must loop without clicks and change smoothly across scene transitions. Browser audio starts after a player gesture. Muting and pause/unfocus settings persist globally. Reduced-flash/motion settings do not disable essential audio or text feedback.

## 9. Map and chapter completion template

An area is authored with dimensions/layers, collision, warps, valid spawn points, named NPCs, trainer encounters, random encounter tables, item placements, music/ambience, time/weather variants, quest hooks, traversal gates, and recovery exits.

A chapter is done only after its mandatory path is reachable, optional loops work, new species are obtainable, quests pay rewards exactly once, all preset teams pass validation, local dialogue changes after events, and the next chapter unlock survives save/reload. Record fastest path, typical completion time, and difficult sections from actual playtests.

First ten-minute test: a new player can understand walking, interaction, choices, battle commands, catching, party information, and healing without reading external instructions. First-gym test: the player has several useful team options, understands the gym strategy, can recover from defeat, and sees a compelling reason to explore the next route.
