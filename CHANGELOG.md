# Changelog

All notable changes to **Star Wars — Pixel Battle Simulator** are documented here.

---

## v1.3.1 — 2026-04-22

### Fixed & Polished
- Config menu font sizes increased on desktop — labels, values and pills were too small to read comfortably
- Slider row spacing increased on desktop (8px) and mobile (10px) — thumb no longer overlaps adjacent rows
- Range input track height increased on mobile for easier touch interaction
- About screen sections aligned to consistent max-width (640px), paragraphs left-aligned instead of centered
- Save button resets to normal appearance immediately when any config change is made
- Config save auto-returns to main menu after brief confirmation

---

## v1.3 — 2026-04-22

### Added
- **Mobile support** — game and menu fill the full viewport; portrait and landscape both work; canvas locks on game start so rotating mid-round doesn't distort the map
- **Fire mode per ship** — Hold Fire / Fire at Will / Return Fire, independently per ship type
  - *Fire at Will* shoots toward target from any heading while holding formation
  - *Return Fire* activates the whole group simultaneously when any member is hit
- **Break trigger per ship** — contact / hit / both / never, independently per ship type
- **Never break** — formation holds indefinitely regardless of contact or damage
- **Config save → auto back** — Save button returns to main menu automatically

### Changed
- Field size presets removed — game always fills the screen
- Formation break trigger and fire mode moved from global to per-ship settings
- Contact range unit changed from px to % of shorter map dimension (default 25%)
- Config max-width capped at 700px — sliders no longer stretch across full screen

### Fixed
- Boundary steering rewritten — proper center-repulsion, scales with map size
- Formation ships now have boundary awareness — no more flying into walls
- Ghost sprites at edges no longer drawn when wrap-around is off
- Formation spawn guaranteed safe via `formationRadius()` pre-calculation

---

## v1.2 — 2026-04-21

### Added
- **Multi-language support** — English, German, Polish; toggle in main menu; saved to `localStorage`
- **Laser Speed** configurable per ship (4–30 px/frame)
- Changelog button links to GitHub — no in-game duplicate to maintain

### Changed
- In-game changelog screen replaced with direct GitHub link

---

## v1.1 — 2026-04-21

### Fixed
- Pause-during-round-end deadlock — respawn uses frame counter instead of `setTimeout`

### Added
- Changelog, About and Load Version screens in main menu
- Version label changed from button to plain text

---

## v1.0 — 2026-04-21 — Initial Release

### Core Features
- 4 pixel-art ship types — X-Wing, A-Wing, TIE Fighter, TIE Bomber
- Inertia-based flight, boid steering, deadlock detection
- Wrap-around world (toroidal) — toggleable; boundary bouncing alternative
- Directional shooting, rotated laser bolts, per-ship laser TTL
- 6 formation types: Delta · Line · Swarm · Wedge · Column · Pincer
- Staggered front-to-back formation dissolution
- Per-ship config: HP · Speed · Turn Rate · Fire Rate · Laser TTL · Damage · Count · Formation
- Scrolling parallax starfield, pixel explosions, HP bars
- 8-bit procedural sound — laser, explosions, fanfare, mute toggle
- Single `.html` file, zero dependencies

---

*May the Force be with you.*
