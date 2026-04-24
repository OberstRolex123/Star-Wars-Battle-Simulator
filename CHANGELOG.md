# Changelog

All notable changes to **Star Wars — Pixel Battle Simulator** are documented here.

---

## v1.3.3 — 2026-04-22

### Added
- **Crawl music** — Star Wars Main Theme starts automatically when the logo appears (5.2s into the crawl)
- **Stop/Start Music button** in the crawl (bottom left, next to Skip) — stops or restarts the theme independently of the game mute
- **♫ Music button** in the in-game HUD — toggle music on/off during a round without affecting sound effects
- **Music toggle in Config** (Visual section) — persisted to `localStorage`; disabling stops any currently playing music

### Fixed
- Music stop button now works reliably — master gain set to 0 immediately on stop; all oscillators terminated with `stop(0)`
- Music no longer replays from the beginning when Stop is pressed — complete rewrite of music engine (`musicPlay` / `musicStop` / `musicIsPlaying`)
- Missing note frequencies `C3` and `F3` added — bass voice was throwing `AudioParam NaN` error and silently failing
- Music volume significantly reduced — master gain 0.25, voice gains at ~20% of original

### Changed
- Play Music button removed from main menu — music accessible via crawl and in-game HUD button only
- `MUSIC_TRACKS` dictionary introduced for future in-game tracks

---

## v1.3.2 — 2026-04-22

### Added
- **Opening Crawl** — authentic Star Wars text crawl with perspective projection, logo animation and blue intro text; appears on first load; re-playable via "Show Intro" button
- **Show Intro button** in main menu
- **Crawl language** — reads saved language from `localStorage` on first load

### Fixed
- Crawl scroll speed frame-rate independent (delta-time, 45px/s)
- CSS animations restart correctly when crawl is re-opened
- "EINE NEUE HOFFNUNG" auto-shrinks font to avoid cropping
- Blue intro text centered on all screen sizes

### Changed
- Mobile menu Portrait: `80vw` buttons (10% side margin), pure `vw` sizing
- Mobile menu Landscape: 2-column grid, `70vw`, `vw`-based font — readable on iPhone 13
- Crawl font scales with viewport (`innerWidth × 104/1200`, capped 32–120px)
- Crawl text strip capped at 1100px — wider screens show more starfield on sides

---

## v1.3.1 — 2026-04-22

### Fixed & Polished
- Config menu font sizes increased on desktop
- Slider row spacing increased — thumb no longer overlaps adjacent rows
- About screen aligned to max-width 640px, paragraphs left-aligned
- Save button resets when unsaved changes are made
- Config save auto-returns to main menu

---

## v1.3 — 2026-04-22

### Added
- **Mobile support** — full viewport, portrait and landscape; canvas locks on game start
- **Fire mode per ship** — Hold Fire / Fire at Will / Return Fire
- **Break trigger per ship** — contact / hit / both / never
- **Config save → auto back**

### Changed
- Field size presets removed — game always fills the screen
- Contact range: px → % of shorter map dimension (default 25%)
- Config max-width 700px

### Fixed
- Boundary steering rewritten with proper center-repulsion
- Formation spawn guaranteed safe via `formationRadius()`
- Ghost sprites hidden when wrap-around is off

---

## v1.2 — 2026-04-21

### Added
- Multi-language support — EN / DE / PL
- Laser Speed configurable per ship
- Changelog links to GitHub

---

## v1.1 — 2026-04-21

### Fixed
- Pause-during-round-end deadlock

### Added
- Changelog, About and Load Version screens

---

## v1.0 — 2026-04-21 — Initial Release

- 4 pixel-art ships, 6 formations, inertia physics, directional shooting
- Wrap-around world, scrolling starfield, HP bars, pixel explosions
- 8-bit procedural sound, per-ship config, localStorage persistence
- Single `.html` file, zero dependencies

---

*May the Force be with you.*
