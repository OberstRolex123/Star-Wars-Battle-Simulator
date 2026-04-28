# Changelog — Star Wars Pixel Battle Simulator

---

## [v1.4.0] — 2026-04-28

### Added
- **Boost System** — 4 directions: `forward`, `back`, `left`, `right`
- Energy tank per ship with continuous regeneration over time; cooldown (0.4s) prevents spam
- **Boost AI** — ships use boost tactically:
  - Incoming laser on collision course → dodge sideways
  - Enemy close + HP < 40% → boost back (create distance)
  - Enemy far + facing target → boost forward (close gap)
  - Enemy aiming at us → dodge sideways
- **Boost Particles** — 12 exhaust particles + glow ring burst on activation
  - Rebel ships: blue tones (`#88ccff`) · Empire ships: green tones (`#88ffcc`)
  - Dedicated particle canvas with `shadowBlur` glow effect
- **Boost Config** — global on/off toggle in Configuration screen
  - Per-ship sliders: Max Energy · Cost/boost · Regen/s · Force
  - Slider blocks dimmed and disabled when boost is globally off
- **Boost Energy Bar** — thin bar below HP indicator (blue = Rebel, green = Empire), only shown when boost enabled
- **Boost Sound** — short sawtooth engine burst on activation, distinct tone per team

---

## [v1.3.3] — 2026-04-21

### Fixed
- Pause during round-end caused permanent deadlock — respawn now uses a frame counter instead of `setTimeout` so it correctly freezes while paused

### Added
- **Changelog screen** — in-game styled with version tags and feature entries (this screen)
- **About / README screen** — ship roster table, formations overview, config summary, tech stack, roadmap
- **Load Version screen** — lists all archived versions with descriptions, opens in new tab
- **Multi-language support** — English, German, Polish; language toggle in main menu (`EN · DE · PL`)
- **Opening Crawl** — full Star Wars intro with starfield, logo animation, music sync at 5.2s, skip button, mute button, language-aware text
- **Star Wars Main Theme** — 4-voice sawtooth synthesis via Web Audio API with reverb; plays during crawl and menu
- **Fire modes** per ship — `Hold Fire` · `Fire at Will` · `Return Fire`
- **Per-ship Break Trigger** — `contact` · `hit` · `both` · `never` configurable individually per ship type
- **Laser Speed** config slider per ship (4–30)
- **Laser TTL** config slider per ship (0.5–5s)
- Scrolling starfield toggle
- Version hint overlay for `file://` protocol users

### Changed
- Config saved under new key `swBattleCfgV5`
- Menu responsive layout: portrait (80vw buttons) and landscape (2-column grid) breakpoints
- `calcGameSize()` fills full viewport dynamically; canvas locked during game, unlocked on return to menu

---

## [v1.0.0] — 2026-04-21 · Initial Release

### Added
- 4 pixel-art ship types — X-Wing, A-Wing, TIE Fighter, TIE Bomber (base64 embedded sprites, SCALE=2)
- 6 formation types per ship — Delta · Line · Swarm · Wedge · Column · Pincer
- Smart formation break — contact range, hit trigger, or both; staggered front-to-back dissolution with configurable delay
- Inertia-based flight physics with deadlock detection and random kick
- Directional shooting — ships must face target within ~15° to fire
- Per-ship configuration — HP (1–50) · Speed · Turn Rate · Fire Rate · Damage · Count (1–20)
- Wrap-around world (toroidal space), toggleable
- Scrolling parallax starfield, toggleable
- 8-bit procedural sound — laser fire (team-distinct), explosions, round-end fanfare, mute toggle
- Score tracking (Rebellion vs Empire, persistent across rounds)
- Config persisted to `localStorage`, survives page reload
- Single HTML file, zero dependencies, no server required
