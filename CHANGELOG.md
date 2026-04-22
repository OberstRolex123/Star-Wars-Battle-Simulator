# Changelog

All notable changes to **Star Wars — Pixel Battle Simulator** are documented here.

---

## v1.3 — 2026-04-22

### Added
- **Mobile support** — game and menu fill the full viewport on any device; portrait and landscape both work; canvas locks to spawn size during a round so rotating mid-game doesn't distort the map
- **Formation fire mode per ship** — each ship type has its own fire mode setting: Hold Fire / Fire at Will / Return Fire, independent of formation break
- **Break trigger per ship** — contact / hit / both / never configurable independently per ship type
- **"Never break" formation option** — ships can hold formation indefinitely regardless of contact or hits
- **Return Fire** — ships remain in formation but fire back the moment any group member is hit; entire group activates simultaneously
- **Fire at Will in formation** — ships shoot toward target from any heading while holding formation (not just when facing within 15°)
- **Config save → auto back** — pressing Save returns to main menu automatically after a brief confirmation

### Changed
- Field size presets (Close Quarters / Open Space / Deep Space) removed — game always fills the screen
- Formation break trigger and fire mode moved from global settings to per-ship settings
- Contact range unit changed from px to % of shorter map dimension — consistent feel across screen sizes; default 25%
- Config max-width capped at 700px — sliders no longer stretch across the full screen
- Save button resets to normal appearance when unsaved changes are made

### Fixed
- Boundary steering rewritten — ships in bounded world no longer fly into walls; repulsion now steers toward map center proportionally to distance from edge
- Formation ships had no boundary awareness — now handled with the same center-repulsion logic
- Ghost sprites at map edges no longer drawn when wrap-around world is disabled
- Formation spawn positions now guaranteed safe — `formationRadius()` pre-calculates extent so no ship spawns outside the playfield

---

## v1.2 — 2026-04-21

### Added
- **Multi-language support** — English, German, Polish; language toggle (EN · DE · PL) in main menu; preference saved to `localStorage`
- **Laser Speed** configurable per ship type (4–30 px/frame); defaults raised to 12–14
- Changelog button links directly to GitHub — no duplicate in-game version to maintain

### Changed
- Changelog removed as in-game screen; replaced with direct GitHub link
- Version bumped to v1.2 across all UI labels

---

## v1.1 — 2026-04-21

### Fixed
- Pause-during-round-end deadlock — respawn now uses a frame counter inside the game loop instead of `setTimeout`

### Added
- **Changelog screen** — in-game styled overlay with version tags and bugfix highlights
- **About / README screen** — ship roster, formations overview, config summary, tech stack, roadmap
- **Load Version screen** — lists archived versions; shows local-server hint when running via `file://`
- Version label in main menu changed from button to plain text

---

## v1.0 — 2026-04-21 — Initial Release

### Ships
- 4 pixel-art ship types — X-Wing, A-Wing, TIE Fighter, TIE Bomber

### Flight Physics
- Inertia-based movement, boid steering, deadlock detection
- Wrap-around world (toroidal) — toggleable
- Boundary bouncing — alternative to wrap

### Combat
- Directional shooting — must face target within ~15°
- Per-ship laser TTL (0.5–5.0 s), double shot for TIE Bomber
- Rotated laser bolts

### Formations
- 6 types: Delta ▲ · Line — · Swarm · Wedge ▼ · Column · Pincer
- Separate anchor zones per ship type
- Staggered front-to-back dissolution

### Configuration
- Per ship: HP (1–50), Speed, Turn Rate, Fire Rate, Laser TTL, Damage, Count (1–20), Formation
- Global: Wrap-around world, Scrolling starfield, Formation flying, Contact range, Stagger delay
- Config saved to `localStorage`

### Visuals & Sound
- Scrolling parallax starfield, pixel explosions, HP bars
- 8-bit procedural sound — laser fire, explosions, round-end fanfare, mute toggle

### Tech
- Single `.html` file, zero dependencies, Vanilla HTML/CSS/JS, Canvas 2D, Web Audio API

---

*May the Force be with you.*
