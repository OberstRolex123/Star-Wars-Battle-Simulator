# Star Wars — Pixel Battle Simulator

> An endless autonomous pixel-art space battle between the Rebellion and the Empire — configurable, exportable, and running entirely in a single HTML file.

Built collaboratively with [Claude](https://claude.ai) over a single session, starting from a blank canvas and evolving into a fully-featured battle simulator with sprites, physics, sound, formations, and mission-ready architecture.

-----

## Live Demo

Open `starwars_battle_v6.html` in any modern browser. No server, no dependencies, no install.

-----

## What it is

Two fleets spawn on opposite sides of the battlefield and fight until one side is eliminated. The round resets automatically and the score ticks up. You watch, configure, and let the Force decide.

-----

## Current Features (v6)

### Visuals

- **Pixel-art sprites** — real top-down pixel art for all four ship types (A-Wing, X-Wing, TIE Fighter, TIE Bomber), each facing and rotating in flight direction
- **Scrolling starfield** — parallax star background with configurable speed, toggleable in settings
- **Rotated laser bolts** — bolts align to the actual flight angle of the firing ship, with a dim trail
- **Pixel explosions** — particle burst on destruction with warm 8-bit color palette

### Flight Physics

- **Inertia-based movement** — ships thrust forward constantly, change direction by rotating, never brake hard
- **Boid steering** — ships seek nearest enemy, maintain separation from allies, avoid walls
- **Wrap-around world** — toroidal space: fly off the top, appear at the bottom (toggleable)
- **Boundary bouncing** — alternative to wrap: ships and lasers are contained within the field
- **Deadlock detection** — if two ships circle each other without progress for ~80 frames, a random kick breaks the loop

### Combat

- **Directional shooting** — ships can only fire when pointing within ~15° of their target; turn rate matters
- **Per-ship laser speed** — configurable per ship type, default 12–14 px/frame
- **Per-ship laser TTL** — laser bolts expire after a configurable time (default 2s), creating effective range
- **Double shot** — TIE Bomber fires two bolts per volley with slight spread

### Formations

- **6 formation types** per ship type, selectable independently:
  - **Delta ▲** — classic V, tip forward (default Rebellion)
  - **Line —** — broad front, all side by side
  - **Swarm** — randomized cluster (default TIE Fighter)
  - **Wedge ▼** — inverted delta, wings forward
  - **Column** — single file
  - **Pincer** — two flanking groups
- **Smart formation break** — formations hold until triggered, then dissolve:
  - **Contact** — breaks when an enemy enters configurable range
  - **Hit** — breaks when the first ship in the group takes damage
  - **Both** — whichever comes first (default)
- **Staggered break** — front ships break first, rear ships follow with configurable frame delay (Option 4)
- **Formation anchor zones** — X-Wings upper-left, A-Wings lower-left, TIE Fighters upper-right, Bombers lower-right

### Sound (8-bit)

- **Laser fire** — square-wave sweep, Rebellion pitches down, Empire pitches up
- **Explosions** — low-pass filtered white noise burst
- **Round end fanfare** — ascending chord for Rebellion win, descending for Empire
- **Mute toggle** in HUD

### Configuration

Everything is configurable per ship type via sliders in the in-game menu:

|Setting             |Range            |
|--------------------|-----------------|
|HP                  |1 – 50           |
|Speed               |0.5 – 4.0        |
|Turn Rate           |0.01 – 0.15      |
|Fire Rate (cooldown)|15 – 120 frames  |
|Laser Speed         |4 – 30 px/frame  |
|Laser TTL           |0.5 – 5.0 seconds|
|Damage              |1 – 10           |
|Count               |1 – 20 ships     |
|Formation           |6 types          |

Global settings:

|Setting            |Options                                             |
|-------------------|----------------------------------------------------|
|Field Size         |Small (480×320) / Medium (680×426) / Large (900×560)|
|Wrap-around world  |On / Off                                            |
|Scrolling starfield|On / Off                                            |
|Formation flying   |On / Off                                            |
|Break trigger      |Contact / Hit / Both                                |
|Contact range      |40 – 300 px                                         |
|Stagger delay      |0 – 60 frames                                       |

Config is saved to `localStorage` and persists between sessions.

-----

## Ship Roster

|Ship       |Team     |HP|Speed|Role                           |
|-----------|---------|--|-----|-------------------------------|
|X-Wing     |Rebellion|6 |1.4  |All-rounder, shields, 4 cannons|
|A-Wing     |Rebellion|2 |2.3  |Interceptor, fast, evasive     |
|TIE Fighter|Empire   |1 |1.6  |Cannon fodder, swarm tactics   |
|TIE Bomber |Empire   |2 |2.2  |Heavy, double shot, 2 HP       |

All values are based on Star Wars lore (Wookieepedia / StarWars.com).

-----

## How to Run

```bash
# Just open the file — no build step, no server needed
open starwars_battle_v6.html
```

Or drop it into any static file host (GitHub Pages, Netlify, etc.).

-----

## Roadmap

The next planned feature is a **campaign mode**:

- Build your fleet by spending credits
- Face predefined Imperial squadrons across escalating mission levels
- Earn credits on victory, regroup and upgrade on defeat
- Ships have fixed base stats; only purchasable upgrades improve them

-----

## Tech Stack

- Vanilla HTML / CSS / JavaScript — zero dependencies
- `<canvas>` 2D API for rendering
- Web Audio API for procedural 8-bit sound
- `localStorage` for config persistence
- All sprites embedded as base64 PNG
- Single `.html` file, ~43KB

-----

## Credits

Built with [Claude Sonnet](https://claude.ai) (Anthropic) through iterative conversation-driven development — sprites, physics, sound, UI, and game logic all designed and debugged in natural language.

Pixel art sprites by the user, extracted and embedded automatically.

*May the Force be with you.*