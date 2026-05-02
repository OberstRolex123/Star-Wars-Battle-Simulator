# Brain System — Documentation

**Star Wars Pixel Battle Simulator v1.5.5**

The Brain system lets you program custom AI behaviour for each ship class using JavaScript. Instead of the built-in AI, your script runs every frame and controls steering, shooting and boosting.

-----

## Device Support

> ⚠️ **The Brain Editor panel is only visible on desktop browsers (viewport width ≥ 900px).**
> On phones it is hidden completely.
> On tablets: iPad Pro (landscape, 1024px) ✓ | iPad Mini / standard iPad portrait ✗

You can still configure Brain scripts via the **Configuration screen** on any device — the live editor panel is a desktop-only convenience feature.

-----

## Two Ways to Use the Brain System

### Option A — Configuration Screen (all devices)

1. Open **Menu → Configuration**
1. Scroll to any ship section (X-Wing, A-Wing, TIE Fighter, TIE Bomber)
1. Find the **Brain** section at the bottom of each ship
1. Toggle Brain on/off, load a preset, or write your script
1. Press **Save** — your script is stored and used in the next simulation

### Option B — Live Brain Editor Panel (desktop only, recommended)

When a simulation is running, activate **Debug Mode** via the `⊕ Debug` button in the HUD. The Brain Editor panel slides in from the right side of the screen.

**Panel features:**

- **Ship dropdown** — switch between X-Wing, A-Wing, TIE Fighter, TIE Bomber
- **Brain toggle** — activate/deactivate Brain for selected class
- **Preset dropdown** — load a ready-made example script
- **Code editor** — full-height textarea, changes take effect on Apply
- **`▶ Apply + New Game`** — saves script, compiles, starts new simulation immediately
- **Error display** — shows compile or runtime errors in red
- **Stats panel** — live comparison table of all runs (see Stats section)
- **Resize** — drag the left edge to make the panel wider or narrower (220–600px)
- **Collapse** — click the `{ }` tab on the left edge to minimize to a thin strip

-----

## How it works

Each ship class (X-Wing, A-Wing, TIE Fighter, TIE Bomber) can have its own Brain script. All ships of that class share the same script — but every individual ship gets its own context with its own position, HP, etc.

When a Brain is active it **completely replaces** the built-in AI for that ship class. If the script throws an error the ship falls back to the normal AI automatically for that frame.

-----

## Script Structure

Your script runs as the **body of a function**. It receives the context variables listed below and must return an object:

```javascript
return {
  steer: 0.0,           // how much to turn (-0.15 to +0.15 per frame)
  shoot: false,         // true = fire this frame
  boost: false          // false | 'forward' | 'back' | 'left' | 'right'
};
```

All three fields are optional — missing fields default to `0` / `false`.

**`steer`** is clamped to the ship’s `turnRate` automatically.

**`shoot`** fires toward the nearest enemy regardless of heading — the engine handles aiming.

**`boost`** only has an effect if the global Boost system is enabled in Configuration.

-----

## Available Variables

### `self` — your ship

|Variable          |Type   |Description                                   |
|------------------|-------|----------------------------------------------|
|`self.x`          |number |X position in pixels                          |
|`self.y`          |number |Y position in pixels                          |
|`self.angle`      |number |Current heading in radians                    |
|`self.vx`         |number |Current velocity X                            |
|`self.vy`         |number |Current velocity Y                            |
|`self.hp`         |number |Current hit points                            |
|`self.maxHp`      |number |Maximum hit points                            |
|`self.cd`         |number |Frames until next shot (0 = can fire)         |
|`self.boostEnergy`|number |Current boost energy (0–max)                  |
|`self.boostReady` |boolean|true = can boost right now                    |
|`self.team`       |string |`'rebel'` or `'empire'`                       |
|`self.type`       |string |`'xwing'` / `'awing'` / `'tie'` / `'tiein'`   |
|`self.laserSpd`   |number |Laser speed in px/frame (useful for lead shot)|

-----

### `foe` — nearest enemy (or `null` if none)

|Variable        |Type  |Description                                         |
|----------------|------|----------------------------------------------------|
|`foe.x`         |number|X position                                          |
|`foe.y`         |number|Y position                                          |
|`foe.angle`     |number|Their heading                                       |
|`foe.vx`        |number|Their velocity X                                    |
|`foe.vy`        |number|Their velocity Y                                    |
|`foe.hp`        |number|Their current HP                                    |
|`foe.maxHp`     |number|Their max HP                                        |
|`foe.dist`      |number|Distance from self in pixels                        |
|`foe.angleTo`   |number|Angle from self to foe (radians)                    |
|`foe.angularVel`|number|Change in foe angle since last frame (for lead shot)|

-----

### `friend` — nearest ally (or `null` if none)

|Variable     |Type  |Description                 |
|-------------|------|----------------------------|
|`friend.x`   |number|X position                  |
|`friend.y`   |number|Y position                  |
|`friend.dist`|number|Distance from self in pixels|

-----

### `foes` — all enemies (array)

Array of all living enemy ships. Same fields as `foe`.

```javascript
// example: find weakest enemy
var weakest = foes.reduce(function(a, b) { return a.hp < b.hp ? a : b; }, foes[0]);
```

-----

### `friends` — all allies (array)

Array of all living friendly ships. Same fields as `friend`.

-----

### `lasers` — incoming enemy lasers (array)

|Variable         |Type  |Description       |
|-----------------|------|------------------|
|`lasers[i].x`    |number|Laser X position  |
|`lasers[i].y`    |number|Laser Y position  |
|`lasers[i].vx`   |number|Laser velocity X  |
|`lasers[i].vy`   |number|Laser velocity Y  |
|`lasers[i].angle`|number|Laser direction   |
|`lasers[i].dist` |number|Distance from self|

Only **enemy** lasers are included.

-----

### `W`, `H` — field dimensions

```javascript
var cx = W / 2; // center of field
var cy = H / 2;
```

-----

### Helper Functions

#### `angleDiff(a, b)`

Returns the shortest angular difference between two angles in radians. Always in range `[-π, +π]`.

```javascript
var offset = angleDiff(foe.angleTo, self.angle); // how much to turn
```

#### `rand(min, max)`

Returns a random float between `min` and `max`.

```javascript
var jitter = rand(-0.1, 0.1);
```

-----

## Presets

Six ready-made scripts are available in the Preset dropdown.

-----

### Default AI

Mimics the built-in AI — faces the nearest enemy, fires when aligned, uses boost to close or retreat.

```javascript
if (!foe) return { steer: 0, shoot: false, boost: false };
var steer = angleDiff(foe.angleTo, self.angle);
if (foe.dist < 65) steer += 0.7;
var b = false;
if (self.boostReady) {
  if (foe.dist < 70 && self.hp / self.maxHp < 0.4) b = 'back';
  else if (foe.dist > 200 && Math.abs(steer) < 0.4) b = 'forward';
}
return {
  steer: steer,
  shoot: Math.abs(angleDiff(foe.angleTo, self.angle)) < 0.26,
  boost: b
};
```

-----

### Aggressive

Steers directly at the nearest enemy at all times. Fires whenever pointing close enough. Boosts forward to close distance.

```javascript
if (!foe) return { steer: 0, shoot: false, boost: false };
var s = angleDiff(foe.angleTo, self.angle);
var shooting = Math.abs(s) < 0.3;
var b = false;
if (self.boostReady && foe.dist > 200) b = 'forward';
return { steer: s, shoot: shooting, boost: b };
```

-----

### Kiter

Maintains an ideal distance of 180px. Retreats when too close, advances when too far. Boosts back if enemy gets very close.

```javascript
var IDEAL = 180;
if (!foe) return { steer: 0, shoot: false, boost: false };
var flee = foe.dist < IDEAL;
var steer = flee
  ? angleDiff(foe.angleTo + Math.PI, self.angle)
  : angleDiff(foe.angleTo, self.angle);
var b = false;
if (self.boostReady && flee && foe.dist < 80) b = 'back';
return { steer: steer, shoot: foe.dist < 280, boost: b };
```

-----

### Flanker

Approaches the enemy from a 90° angle instead of head-on. More evasive and harder to hit.

```javascript
if (!foe) return { steer: 0, shoot: false, boost: false };
var flank = foe.angleTo + Math.PI * 0.5;
var steer = angleDiff(flank, self.angle);
var shooting = Math.abs(angleDiff(foe.angleTo, self.angle)) < 0.45;
var b = false;
if (self.boostReady && foe.dist > 220) b = 'forward';
return { steer: steer, shoot: shooting, boost: b };
```

-----

### Coward

Always runs away. Only fires when the enemy is behind it (within 30° of ship’s back).

```javascript
if (!foe) return { steer: 0, shoot: false, boost: false };
var flee = angleDiff(foe.angleTo + Math.PI, self.angle);
var behindAngle = Math.abs(angleDiff(foe.angleTo, self.angle + Math.PI));
var b = false;
if (self.boostReady) b = 'forward';
return { steer: flee, shoot: behindAngle < 0.3, boost: b };
```

-----

### Lead Shot (Vorhalten)

Calculates where the enemy will be when the laser arrives. Uses `foe.angularVel` for enemies that are turning.

```javascript
if (!foe) return { steer: 0, shoot: false, boost: false };

var bulletSpeed = self.laserSpd || 12;
var leadTime = foe.dist / bulletSpeed;

// Predict enemy position accounting for their turning rate
var leadAngle = foe.angleTo + foe.angularVel * leadTime * 0.8;
var steer = angleDiff(leadAngle, self.angle);
var shooting = Math.abs(angleDiff(leadAngle, self.angle)) < 0.2;

var b = false;
if (self.boostReady) {
  if (foe.dist > 250) b = 'forward';
  else if (Math.abs(steer) > 0.8) b = 'forward';
}

return { steer: steer, shoot: shooting, boost: b };
```

-----

## Writing Your Own Brain

A minimal working script:

```javascript
if (!foe) return { steer: 0, shoot: false, boost: false };

var steer = angleDiff(foe.angleTo, self.angle);
var shoot = Math.abs(steer) < 0.3 && self.cd === 0;

return { steer: steer, shoot: shoot, boost: false };
```

**Tips:**

- Always check `if (!foe)` first — there may be no enemies alive
- `angleDiff(foe.angleTo, self.angle)` is the most useful single expression
- Keep scripts simple — complex logic may hit the 2ms timeout
- Use `var` instead of `let`/`const` for maximum browser compatibility
- `steer` is added to the ship’s heading each frame — small values (0.05–0.15) produce smooth turns
- `self.laserSpd` gives you the exact laser speed for lead shot calculations
- `foe.angularVel` is the change in the enemy’s angle since last frame — useful for predicting curved paths

-----

## Sandboxing & Safety

- Scripts run inside `new Function()` — **no access** to DOM, `localStorage`, canvas, or global game variables
- Each frame is wrapped in `try/catch` — a crash only affects that ship for that frame
- Scripts are **time-limited to 2ms** — if too slow, the ship falls back to normal AI
- Error messages appear below the textarea in the editor and as `ERR` on the ship in Debug Mode
- Brain scripts are saved in `localStorage` under `swBrainV1` — **separate from main config** so Reset All does not wipe them

-----

## Debug Mode

Debug Mode is a development tool for observing and tuning Brain scripts in real time.

**How to activate:** Press the `⊕ Debug` button in the HUD during a simulation. The button is always visible when a simulation is running.

> ⚠️ Debug Mode visuals only appear for ships with an **active Brain**. Ships using the built-in AI show nothing.

### What it shows (per Brain-controlled ship)

|Visual                     |Description                                      |
|---------------------------|-------------------------------------------------|
|Blue/green arrow           |Current steering direction (40px, with arrowhead)|
|Dashed red line + arrowhead|Line from ship to its current target enemy       |
|Yellow arrow               |Boost direction when a boost is triggered        |
|`ERR` text                 |Script crashed this frame                        |

### Speed Control

When Debug Mode is on, a **speed slider** appears in the HUD:

|Setting|Effect                           |
|-------|---------------------------------|
|`0x`   |Frozen — step controls appear    |
|`0.1x` |Very slow — almost frame-by-frame|
|`0.25x`|Quarter speed                    |
|`0.5x` |Half speed                       |
|`0.75x`|Slightly slowed                  |
|`1.0x` |Normal speed                     |
|`2x`   |Double speed                     |
|`5x`   |5× speed                         |
|`10x`  |10× speed                        |


> Note: A separate **global speed slider** (without Debug) is always available in the HUD bottom bar for normal speed adjustments during any simulation.

### Step Mode (Speed = 0x)

When frozen, four step buttons appear:

`◀◀ -10` — jump 10 frames back  
`◀ -1` — one frame back  
`+1 ▶` — one frame forward  
`+10 ▶▶` — jump 10 frames forward

- Going **back** restores a previously recorded frame — nothing is recalculated
- Going **forward** while in recorded history replays the **exact same frames**
- Going **forward** past the last recorded frame simulates a new tick and records it
- Frame counter `128/450` turns **orange** when viewing the past
- Up to **600 frames (~10 seconds)** are stored while Debug is active

-----

## Stats Panel

The Stats panel appears at the bottom of the Brain Editor (desktop only).

**How runs work:**

- Stats accumulate during a simulation for all 4 ship classes simultaneously
- When you press `▶ Apply + New Game`, the current run is **frozen as R1, R2, R3…**
- The new simulation starts fresh with a `Live` column
- You can switch between ship classes in the dropdown to see their individual stats

**What is tracked per ship class:**

|Stat         |Description                                 |
|-------------|--------------------------------------------|
|Shots fired  |Total shots this run                        |
|Hit rate     |Shots that hit an enemy / total shots       |
|1st-shot rate|How often the first shot of a life connected|
|Kills        |Enemies destroyed by this ship class        |
|Deaths       |How often ships of this class were destroyed|
|Ø Kill time  |Average time from spawn to making a kill    |
|Boosts used  |Total boost activations                     |

**Trend arrows:**

- `▲` green — improved vs previous run
- `▼` red — worse vs previous run

**Reset** clears all frozen runs and live data for all ship classes.

> Stats are tracked for **all ship classes** regardless of whether a Brain is active — the normal AI is also tracked.

-----

## Example: Laser-Dodging Script

```javascript
// Dodge incoming lasers, then attack
if (!foe) return { steer: 0, shoot: false, boost: false };

// Check for incoming lasers
var dodge = false;
for (var i = 0; i < lasers.length; i++) {
  var l = lasers[i];
  if (l.dist < 100) {
    var laserToMe = Math.atan2(self.y - l.y, self.x - l.x);
    var diff = Math.abs(angleDiff(laserToMe, l.angle));
    if (diff < 0.4) {
      dodge = true;
      break;
    }
  }
}

if (dodge && self.boostReady) {
  return { steer: angleDiff(foe.angleTo, self.angle), shoot: false, boost: 'left' };
}

var steer = angleDiff(foe.angleTo, self.angle);
return {
  steer: steer,
  shoot: Math.abs(steer) < 0.3,
  boost: (self.boostReady && foe.dist > 200) ? 'forward' : false
};
```
