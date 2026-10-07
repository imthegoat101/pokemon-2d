# Pokémon Aster — UI, UX, and PC controls

Status: proposed v0.1. Companion to the [game design](superpowers/specs/2026-10-07-pokemon-aster-design.md). All dimensions are design targets to validate at 720p and 1080p, not finished artwork.

## 1. Visual system

The game should look like a carefully illustrated Pokémon adventure with DS-era pixel detail and a comfortable PC interface. Environment sprites remain pixel art. Text and menu panels render crisply at display resolution. Do not put every UI element on the 480×270 world grid.

| Token | Proposal | Use |
| --- | --- | --- |
| Ink | `#192B3A` | Body text and dark HUD backing |
| Paper | `#FFF8EA` | Main panel surfaces |
| Teal | `#176F74` | Focus, active tabs, primary actions |
| Gold | `#B77712` | Badge/reward accents, never sole status indication |
| Mist | `#D7E7E6` | Dividers and secondary surfaces |
| Danger | `#A72E3E` | Destructive actions with explicit text/icon |
| Night | `#111D2D` | Optional dark menu theme |

Use one licensed, highly readable UI typeface with Latin/Portuguese characters. Reserve pixel lettering for short decorative labels. Proposed base type: 18 px at 720p, configurable scale 100–150%. Minimum 40 px primary hit targets at 720p; controls can grow with UI scale. Test actual contrast with final colors, including disabled and selected states.

Panels have subtle pixel-compatible borders, modest corner rounding, and restrained shadows. Do not blur the world heavily behind every menu. Transition duration target: 120–180 ms; reduced-motion mode uses immediate state changes or short fades. Focus uses outline plus icon or underline, never color alone.

## 2. Input model

| Action | Keyboard default | Controller default | Pointer |
| --- | --- | --- | --- |
| Move/navigate | WASD or arrows | D-pad / left stick | Menus only; no click-to-walk |
| Confirm/interact | Enter or E | South face button | Left click |
| Back/cancel | Escape or Backspace | East face button | Back button; right click in safe menu contexts |
| Main menu | Tab | Start/Menu | HUD menu button |
| Run | Shift, hold/toggle option | Left shoulder, hold/toggle | Setting only |
| Registered field item | R | Right shoulder | Context HUD button |
| Map | M | View/Select | Menu entry |
| Quest journal | J | Main menu entry | Menu entry |
| Party | P | Main menu entry | Menu entry |
| Pokédex | K | Main menu entry | Menu entry |
| Cycle tab | Q/E within a tabbed menu | Shoulders | Click tab |
| Battle detail | I | North face button | Info icon |
| Battle action | 1–4 for visible choices | Navigate and confirm | Click choice |

All bindings remappable. When E is used for tab cycling in a tabbed menu, Enter remains confirm; the context footer displays this. Rebinding detects conflicts within each input context and offers swap/replace/cancel. Gamepad prompts follow the active device and include a generic symbol fallback. Disconnecting a controller switches to keyboard prompts without losing focus.

One active input context owns commands: overworld, menu, dialog, battle choice, target picker, modal, or text entry. Opening/closing a context consumes the triggering input. Modals block underlying actions. Releasing a held movement key on a focus change cannot leave the player walking indefinitely.

The world pauses during menus and text dialogue. Battle presentation pauses when the window loses focus, except explicitly user-enabled background behavior. Browser-reserved shortcuts are not rebound by default. Fullscreen must follow a direct player gesture and fail gracefully.

## 3. Screen inventory

| Screen | Main content | Required secondary/error states |
| --- | --- | --- |
| Boot/loading | Logo, actual load progress, active pack | Network failure, missing pack, unsupported renderer, retry |
| Title | Animated town scene; Continue, New Game, Profiles, Settings, Credits | No profiles, damaged save, update migration required |
| Profile selector | Three cards: trainer, play time, location, badges, last save | Empty, valid, recovery available, failed storage |
| New game | Character, name, pronouns, preset, control summary | Invalid name, profile overwrite confirmation |
| Overworld HUD | Location banner, optional quest, contextual action, menu icon | Unsaved indicator, inactive window, area loading |
| Dialogue | Speaker portrait/name, text, choices, continuation cue | Long text, revisited dialogue, no portrait |
| Main menu | Party, Bag, Pokédex, Map, Journal, Trainer, Save, Settings | Context-disabled entries with reason |
| Party | Six slots with sprite, level, HP, status, held item | Empty slots, fainted, selected target, full party |
| Pokémon summary | Overview, stats, moves, details/ribbons | Unknown ability information, move replacement |
| Bag | Categories, list, quantities, detail/action panel | Empty category, unusable item, insufficient quantity |
| Storage | Party strip, box grid, detail panel, box navigation | Full box, search empty, last-party-member restriction |
| Pokédex | Regional list, seen/caught flags, filters | Unseen silhouettes, seen-only details, no matches |
| Pokédex entry | Sprite, type, research text, habitat, evolution | Unknown habitat, alternate source hint |
| Region map | Map, player, visited nodes, travel nodes, quest marker | Locked travel, unknown area, invalid selection |
| Quest journal | Active/completed lists, objective, progress, reward | No quests, completed reward unclaimed |
| Trainer card | Avatar, name, badges, statistics, achievements | Badge not yet earned |
| Save/recovery | Active profile, manual save, autosave, backups, export/import | Write failed, migration failed, incompatible import |
| Settings | Controls, video, audio, gameplay, accessibility | Invalid binding, unsupported fullscreen, reset confirm |
| Shop | Buy/sell tabs, list, effect, money, quantity | Cannot afford, cannot sell, stack limit |
| Center service | Heal, storage, reminder, nickname | No service needed, illegal nickname |
| Battle | Combatants, HUD, move/action panel, event text | Forced switch, targets, no PP, no legal action |
| Battle results | EXP progression, money, levels, rewards | Move learn, evolution, capture naming/storage |
| Evolution | Old/new sprite, animation, accept/cancel, resulting entry | Canceled, item consumed only on confirmed result |
| Trade | Requested/offered species, selected instance, result | Ineligible Pokémon, cancel, last party member |
| Circuit lobby/results | Team selection, rules, stage, rewards | Ineligible party, defeat, completed run |
| Credits | Contributors, asset credits, music, return action | First completion transition and saved postgame state |

Every usable control supports visible keyboard focus. Every disabled action explains why, rather than silently ignoring input.

## 4. Title and onboarding

Title composition: the world artwork takes roughly two-thirds of the viewport; the primary action stack sits on the right. Continue includes profile name, location, and badge count. If no valid save exists, New Game is primary. Settings are accessible before play so controls and audio can be adjusted immediately.

New game uses a four-step compact flow: profile → trainer → difficulty → begin. Provide a final recap and a brief description of the preset. Limit name to 12 displayed grapheme clusters; allow ordinary accents and spaces; prevent control characters and blank names. A selected occupied profile requires a separate overwrite modal that identifies the trainer and progress; cancel is initially focused.

First ten minutes teach through play: walk and interact, choose a starter, fight a guided rival battle, receive the journal and map, see a catching demonstration, catch voluntarily, and use a Center. Tutorial flags stop tips repeating on reload. A returning player can select condensed tips; do not skip required story rewards.

## 5. Overworld and dialogue layout

Keep the world dominant. A location banner appears for two seconds after an area change and then disappears. The upper-right objective displays at most two lines, with optional collapse. The bottom-right action prompt appears only when relevant. No permanent minimap, six-member party strip, or quest checklist covering the environment.

Dialogue panel: bottom of screen, about 28% of height at default scale, with a 96 px portrait region, readable text region, speaker heading, and continuation cue. Two-to-four choices appear above it; longer choice lists scroll inside a constrained panel. Confirm finishes the current text reveal before advancing; instant text still requires a distinct confirm to advance. NPC dialogue restores the player's orientation on exit.

Important dialogue has a local recent-text log. It stores story text, not an unbounded copy of every combat event. Choices influence small reactions and quest resolutions; the campaign does not branch into multiple large narratives.

## 6. Main menu and party

Main menu is a side panel occupying about 32% width at 1080p, with trainer summary and 8 destinations. World remains visible and paused. Subscreens become full panels when needed. Back always returns to the same selected parent entry.

Party screen: left 55% holds six two-column cards; right 45% shows the selected Pokémon's summary and actions. Cards show nickname/species, level, HP number/bar, status icon/text, and held item. Actions: Summary, Move/reorder, Give/take item, Use item, Set lead. No field-move menu; licensed traversal is contextual in the world.

Keyboard reordering: choose Move, navigate destination, confirm. Mouse drag is an optional accelerator with a clear drop preview. Both paths use the same validated party command. The last usable party member cannot be deposited while away from a safe healing flow; storage must never leave an invalid empty party.

Summary tabs: Overview (type, ability, nature, capture), Stats (six values with optional IV/EV explanations), Moves (power/accuracy/PP/category), and Details (friendship and milestones). Hidden enemy data is not exposed through this screen.

Move learning shows the new move and all four existing moves together. Compare information without closing the dialog. Choose a slot, confirm replacement, or decline. A reminder of free relearning prevents regret. Advancing a results animation cannot implicitly decline a move.

## 7. Battle layout and flow

Singles composition at 16:9:

| Region | Relative placement | Content |
| --- | --- | --- |
| Enemy field | Upper-right central field | Opponent front sprite, terrain effects |
| Enemy HUD | Upper-left, approximately 30% width | Name/species, level, HP bar, visible status |
| Player field | Lower-left central field | Player back sprite |
| Player HUD | Mid-right above command panel | Name, level, numeric HP/bar, EXP, status |
| Event strip | Bottom-left, about 58% width and 20% height | Current event text and optional battle log |
| Commands | Bottom-right, about 40% width and 20% height | Fight, Pokémon, Bag, Run |

Fight replaces command buttons with a 2×2 move grid. Each cell contains move name, type icon/label, PP, and effectiveness hint when enabled. The selected move opens a compact detail strip for power, accuracy, category, and description. Effectiveness hints account only for known information and are not guaranteed-damage predictions.

Doubles reduce sprite scale, arrange allies lower-left and opponents upper-right, and give each an individually associated HUD. Select actions for both allies, then confirm a turn summary. Back can revise choices before commit. The target picker highlights eligible targets and describes spread attacks. Do not overload singles shortcuts to select targets without a visible prompt.

Battle input follows states: intro → choice → optional target → confirm/commit → event playback → forced replacement or results → next choice/exit. Only choice states accept actions. Button labels cannot remain enabled during playback. A concise indicator identifies whose action is executing.

Animations: HP updates after the hit event, not before; fainting follows engine faint events; status appears alongside its explanation. Repeated effects can group text without dropping meaningful information. Speed settings at 1×, 2×, and 4× shorten presentation only. An animations-off mode preserves flashes-free hit feedback and all readable outcomes.

Battle log holds the latest 50 semantic events with turn numbers. Weather, terrain, screens, and hazards have an expandable field-status summary. Run is visibly unavailable in trainer battles. An unusable ball or healing item explains its restriction before item consumption.

Bag use in battle enters a modal overlay with relevant categories and valid targets. Cancel returns to the command panel unchanged. A selected item is consumed only when its action commits legally. Forced-switch UI cannot choose fainted Pokémon; a party wipe bypasses the picker and enters defeat results.

Capture results show the caught Pokémon, Pokédex update, nickname option, and destination. Full party offers Send to box or Replace party member; cancellation defaults to a named box destination. Capture is already secured before nickname entry. Closing the window cannot lose it after a successful save.

## 8. Storage, bag, and shops

Storage uses a 6×5 box grid, left party column, top box title/arrows, and right details. Search/filter by species, nickname, type, and level; favorite/lock flags protect valued Pokémon. Multi-select and mass release are deferred. Release requires a modal naming the Pokémon and is blocked for favorites/locks, unique protected story gifts, and last party member. Destructive cancel starts focused.

Bag has a left category rail, middle item list, and right detail/actions. Preserve selection and scroll when returning from a target picker. Empty lists explain how to acquire items. Quantity, maximum stack, and current ownership are visible.

Shop quantity control supports arrows, typed quantity, and Max. Display total cost and money after purchase before confirmation. Selling uses the same interaction, but excludes key items, unique quest objects, and registered progression items. Purchase failures retain the selection and explain the problem.

## 9. Pokédex, map, and journal

Pokédex filters for seen/caught, type, habitat, and evolution family. Unseen entries reveal a silhouette and number only; seen entries reveal basic encounter information; caught entries reveal full descriptions and available evolution guidance. Completion counts use the 240-species regional roster, not every form as a separate species.

Map supports keyboard node navigation, pointer selection, zoom steps, and legend. Travel is possible only between unlocked nodes and only outside locked story situations. Show the destination and arrival point before confirming. An optional quest marker identifies an area, not an undiscovered exact hidden-item tile.

Journal lists active quests before completed quests. Details show why the objective matters, measurable progress, relevant NPC/location, and exact rewards. Track/untrack is a single action. Completed-but-unclaimed rewards are distinct from claimed quests. No red notification dots for repeatable optional activities.

## 10. Saves and settings

The save screen identifies profile and last successful save, plus an explicit Save now action. Export is always available when a valid in-memory snapshot exists. Import first previews trainer, badges, play time, schema compatibility, and target profile. Replacing a profile requires confirmation and preserves a recovery copy.

Do not use a spinning icon as the only indication of a failed write. Show: "Progress could not be saved. Retry or export a backup." Provide retry and export without dismissing the message automatically. Storage failure never silently wipes progress. On browser first save, a short note explains browser-local storage and backup export.

Settings categories:

- Video: fullscreen/windowed where supported, UI scale, effects quality, pixel scaling policy, brightness.
- Audio: master/music/ambience/SFX, mute when unfocused.
- Controls: device, bindings, autorun, controller deadzone.
- Gameplay: text speed, animation speed, effectiveness hints, Switch/Set when permitted, shared EXP when permitted, condensed tutorials.
- Accessibility: reduced motion, reduced flashes, high-contrast focus, status labels, color-independent icons.

Difficulty changes go through a healing-point interaction; the settings page shows current rules and where to change them. Global settings survive profile switching; campaign-specific gameplay rules belong to the profile.

## 11. UI acceptance criteria

At 1280×720 and 1920×1080, all screens fit at default scale and enlarged text has a functional scroll/compact fallback. No important content is clipped. Keyboard alone can create a save, catch, rename, reorder, buy, deposit, learn a move, navigate a doubles turn, finish a gym, export, and import.

Back behavior is predictable; all popups return focus to their invoking control. Type/status indicators pass a grayscale review. Enemy HP never displays fabricated numeric information. No input passes through a modal or triggers two actions. Screens are verified in populated, empty, disabled, loading, and error states.

Before producing all UI art, build exact interactive layouts for battle, party, storage, and dialogue. A static mockup cannot prove focus, controller behavior, or move targeting; those need a playable UI review during the first-gym slice.
