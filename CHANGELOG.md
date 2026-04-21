# Changelog

All notable changes to **Star Wars — Pixel Battle Simulator** are documented here.

---

## v1.2 — 2026-04-21

### Added
- **Multi-language support** — English, German, Polish; language toggle (EN · DE · PL) in main menu; preference saved to `localStorage`
- **Laser Speed** configurable per ship type (4–30 px/frame); defaults raised to 12–14 for snappier combat
- Changelog button now links directly to this file on GitHub — no duplicate in-game version to maintain

### Changed
- Changelog removed as an in-game screen; replaced with direct GitHub link
- Version bumped to v1.2 across all UI labels

---

## v1.1 — 2026-04-21

### Fixed
- Pause-during-round-end deadlock — respawn now uses a frame counter inside the game loop instead of `setTimeout`, so it correctly freezes while paused and resumes cleanly on unpause

### Added
- **Changelog screen** — in-game styled overlay with version tags, feature entries (★) and bugfix highlights (✓)
- **About / README screen** — ship roster table, formations overview, config summary, tech stack and roadmap
- **Load Version screen** — lists archived versions with descriptions; opens in new tab; shows a local-server hint when running via `file://`
- Version label in main menu changed from button to plain text

---

## v1.0 — 2026-04-21 — Initial Release

### Ships
- 4 pixel-art ship types with top-down sprites, pivot calibration and rotation
  - **X-Wing** (Rebellion) — 6 HP, speed 1.4, all-rounder
  - **A-Wing** (Rebellion) — 2 HP, speed 2.3, interceptor, evasive
  - **TIE Fighter** (Empire) — 1 HP, speed 1.6, swarm cannon fodder
  - **TIE Bomber** (Empire) — 2 HP, speed 2.2, double shot

### Flight Physics
- Inertia-based movement — thrust forward, rotate to steer, no hard braking
- Boid steering — seek nearest enemy, maintain ally separation
- Deadlock detection — frame-sampled position check every 80 frames; random kick breaks circular pursuit loops
- Wrap-around world (toroidal space) — toggleable
- Boundary bouncing — alternative to wrap; ships and lasers contained within field

### Combat
- Directional shooting — ships must face target within ~15° to fire
- Per-ship laser TTL (0.5–5.0 seconds)
- TIE Bomber fires 2 bolts per volley with slight angular spread
- Rotated laser bolts — bolts align to actual firing angle with dim trail

### Formations
- 6 formation types per ship type: **Delta ▲ · Line — · Swarm · Wedge ▼ · Column · Pincer**
- Separate anchor zones — X-Wings upper-left, A-Wings lower-left, TIE Fighters upper-right, Bombers lower-right
- Smart formation break: **Contact** / **Hit** / **Both** (configurable)
- Staggered dissolution — front ships break first, rear follow with configurable frame delay

### Configuration
- Per ship: HP (1–50), Speed, Turn Rate, Fire Rate, Laser TTL, Damage, Count (1–20), Formation
- Global: Field size (Small / Medium / Large), Wrap-around world, Scrolling starfield, Formation flying, Break trigger, Contact range, Stagger delay
- Config saved to `localStorage`, persists between sessions

### Visuals
- Scrolling parallax starfield — toggleable, runs on menu screen too
- Pixel explosion particles on destruction
- HP bars per ship

### Sound (8-bit, procedural)
- Laser fire — square wave sweep; Rebellion pitches down, Empire pitches up
- Explosions — low-pass filtered white noise burst
- Round-end fanfare — ascending chord (Rebellion win) / descending (Empire win)
- Mute toggle in HUD

### Tech
- Single `.html` file, ~80KB, zero dependencies
- Vanilla HTML / CSS / JavaScript, Canvas 2D API, Web Audio API, localStorage
- All sprites embedded as base64 PNG

---

*May the Force be with you.*
