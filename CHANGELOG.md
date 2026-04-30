# Changelog — Star Wars Pixel Battle Simulator

---

## [v1.5.5] — 2026-04-30

### Added
- **Run comparison stats** — each `Apply + New Game` freezes current stats as a named run (R1, R2, R3…); horizontal scrollable table shows all runs side by side; auto-scrolls to latest; `▲▼` trend arrows (green/red) vs. previous run; Live column highlighted in blue
- **Stats for all ship classes** — stats tracked for all 4 classes regardless of Brain active state; switch between classes in dropdown to compare
- **Ship class label in stats header** — `⚡ Stats  X-Wing` shows which class you are currently viewing
- **Lead Shot preset** — calculates intercept point using `foe.dist / self.laserSpd` and `foe.angularVel`; available in all preset dropdowns
- **Brain context extended** — `foe.angularVel`, `foe.vx`, `foe.vy`, `self.laserSpd` now available in Brain scripts
- **Full i18n for debug/brain/stats UI** — all HUD buttons, Brain panel labels, stats table headers now translated in DE and PL
- **Crawl speed increased** — scroll speed raised from 45 → 61 px/s (+35% total)
- **`crawl-fade-top` removed** — legacy gradient overlay from old 2D crawler; no longer needed with perspective 3D crawler

### Fixed
- Stats not updating — `bp-stats-body` element was removed from DOM when `scroll.innerHTML=''` was called; now writes directly to scroll container
- Stats labels showing English in DE/PL — wrong values were copied into DE/PL language blocks
- Shot counter only counting first shot per ship — `_shotFired` flag was never reset; now counts every shot
- `ReferenceError: can't access LANGS before initialization` — `t()` was called inside the `LANGS` constant definition
- Starfield missing after LANGS error — script abort prevented `initStars()` from running
- Brain panel black area on exit to menu — `bpUpdateGameWidth()` returned early when `gameRunning=false`; now also resizes `#app` in menu mode
- Debug button not appearing — `style="display:none"` hardcoded in HTML; removed

### Changed
- Stats table replaced simple single-column layout with multi-run comparison table
- Stats reset now clears all runs for all ship classes

---

## [v1.5.4] — 2026-04-29

### Added
- **Stats system** — tracks per ship class: shots fired, hit rate, first-shot rate, kills, deaths, Ø kill time, boosts used; accumulates across runs
- **Global speed slider** — always visible in HUD (when Debug off); `0.1x · 0.25x · 0.5x · 0.75x · 1x · 1.25x · 1.5x · 2x · 5x · 10x`
- **Countdown** — `3 · 2 · 1 · FIGHT!` animation before each game
- **Live Brain Editor Panel** — resizable side panel (desktop ≥900px); drag handle; collapse tab; ship selector; preset loader; full-height textarea; `▶ Apply + New Game`; error display
- **Debug target arrow** — filled arrowhead on target line
- **Debug speed slider extended** — now includes `2x · 5x · 10x`; 1x at position 5/8
- **HUD font scaling** — `clamp()` for responsive sizing
- **Custom crawl story** — Operation: Iron Wing; Combat Simulation Division; Rebel engineer narrative in EN/DE/PL
- **Star Wars SVG logo** in menu replaces text title
- **`New Simulation`** replaces `New Game` in all languages

### Fixed
- Black area left of screen when Brain panel open
- Brain preset reset to Default AI after Apply
- Drag handle unresponsive
- Step mode cursor at frame 0 instead of latest
- Pause overlay replaced with in-HUD toggle

---

## [v1.5.0–1.5.3] — 2026-04-28

### Added
- **Brain System** — custom JS AI per ship class; sandboxed `new Function()`; 2ms timeout; fallback to normal AI
- **5 Brain Presets** — Default AI · Aggressive · Kiter · Flanker · Coward
- **Brain Context API** — `self`, `foe`, `friend`, `foes`, `friends`, `lasers`, `W`, `H`, `angleDiff`, `rand`
- **Debug Mode** — steering arrow, target line with arrowhead, boost arrow, ERR indicator
- **Debug Speed Slider** — `0x` to `1.0x` slow motion
- **Step Mode** — bidirectional 600-frame buffer; replay recorded frames; simulate new frames past end of buffer
- **`BRAIN_DOCUMENTATION.md`** — full API reference

---

## [v1.4.0] — 2026-04-28

### Added
- **Boost System** — 4 directions, energy tank with regen, cooldown
- **Boost AI** — tactical dodging, retreat, gap-closing
- **Boost particles** — exhaust + glow ring; Rebel blue · Empire green
- **Boost config** — global toggle + per-ship sliders

---

## [v1.3.3] — 2026-04-21

### Fixed
- Pause during round-end permanent deadlock

### Added
- Changelog · About · Load Version screens
- Multi-language — EN · DE · PL
- Opening Crawl with logo animation and music
- Star Wars Main Theme — 4-voice Web Audio synthesis
- Fire modes and per-ship Break Trigger

---

## [v1.0.0] — 2026-04-21 · Initial Release

### Added
- 4 pixel-art ship types — X-Wing · A-Wing · TIE Fighter · TIE Bomber
- 6 formation types — Delta · Line · Swarm · Wedge · Column · Pincer
- Inertia-based physics, per-ship config, 8-bit sound
- Single HTML file, zero dependencies
