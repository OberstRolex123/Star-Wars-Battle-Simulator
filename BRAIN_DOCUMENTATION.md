# Brain System — Documentation
**Star Wars Pixel Battle Simulator v1.5.0**

The Brain system lets you program custom AI behaviour for each ship class using JavaScript. Instead of the built-in AI, your script runs every frame and controls steering, shooting and boosting.

---

## How it works

Each ship class (X-Wing, A-Wing, TIE Fighter, TIE Bomber) can have its own Brain script. All ships of that class share the same script — but every individual ship gets its own context with its own position, HP, etc.

When a Brain is active it **completely replaces** the built-in AI for that ship class. If the script throws an error the ship falls back to the normal AI automatically for that frame.

---

## The Editor

Open **Configuration → any ship section → Brain**.

- **Toggle** — activate or deactivate the Brain for this ship class
- **Preset dropdown** — load a ready-made example script (see Presets below)
- **Reset** — clear the script and deactivate the Brain
- **? Docs** — opens this page
- **Textarea** — write your script here. Changes take effect immediately (no save needed).
- **Error line** — if your script crashes, the error message appears here in red

Scripts are saved automatically to `localStorage` under the key `swBrainV1` — separate from the main config so **Reset All** does not wipe your scripts.

---

## Script structure

Your script runs as the **body of a function**. It receives the context variables listed below and must return an object:

```javascript
return {
  steer: 0.0,           // how much to turn (-0.15 to +0.15 per frame)
  shoot: false,         // true = fire this frame
  boost: false          // false | 'forward' | 'back' | 'left' | 'right'
};
```

All three fields are optional — missing fields default to `0` / `false`.

**`steer`** is clamped to the ship's `turnRate` automatically. You don't need to worry about turning too fast.

**`shoot`** fires toward the nearest enemy regardless of heading — the engine handles aiming. You only decide *when* to shoot.

**`boost`** only has an effect if the global Boost system is enabled and the ship has enough energy.

---

## Available variables

### `self` — your ship

| Variable | Type | Description |
|---|---|---|
| `self.x` | number | X position in pixels |
| `self.y` | number | Y position in pixels |
| `self.angle` | number | Current heading in radians |
| `self.vx` | number | Current velocity X |
| `self.vy` | number | Current velocity Y |
| `self.hp` | number | Current hit points |
| `self.maxHp` | number | Maximum hit points |
| `self.cd` | number | Frames until next shot (0 = can fire) |
| `self.boostEnergy` | number | Current boost energy (0–max) |
| `self.boostReady` | boolean | true = can boost right now |
| `self.team` | string | `'rebel'` or `'empire'` |
| `self.type` | string | `'xwing'` / `'awing'` / `'tie'` / `'tiein'` |

---

### `foe` — nearest enemy (or `null` if none)

| Variable | Type | Description |
|---|---|---|
| `foe.x` | number | X position |
| `foe.y` | number | Y position |
| `foe.angle` | number | Their heading |
| `foe.hp` | number | Their current HP |
| `foe.maxHp` | number | Their max HP |
| `foe.dist` | number | Distance from self in pixels |
| `foe.angleTo` | number | Angle from self to foe (radians) |

---

### `friend` — nearest ally (or `null` if none)

| Variable | Type | Description |
|---|---|---|
| `friend.x` | number | X position |
| `friend.y` | number | Y position |
| `friend.dist` | number | Distance from self in pixels |

---

### `foes` — all enemies (array)

Array of all living enemy ships. Same fields as `foe`.

```javascript
// example: find weakest enemy
const weakest = foes.reduce((a, b) => a.hp < b.hp ? a : b, foes[0]);
```

---

### `friends` — all allies (array)

Array of all living friendly ships. Same fields as `friend`.

---

### `lasers` — incoming enemy lasers (array)

| Variable | Type | Description |
|---|---|---|
| `lasers[i].x` | number | Laser X position |
| `lasers[i].y` | number | Laser Y position |
| `lasers[i].vx` | number | Laser velocity X |
| `lasers[i].vy` | number | Laser velocity Y |
| `lasers[i].angle` | number | Laser direction |
| `lasers[i].dist` | number | Distance from self |

Only **enemy** lasers are included — friendly fire is not listed.

---

### `W`, `H` — field dimensions

```javascript
// center of the field
const cx = W / 2;
const cy = H / 2;
```

---

### Helper functions

#### `angleDiff(a, b)`
Returns the shortest angular difference between two angles in radians.
Result is always in the range `[-π, +π]`.

```javascript
// how far off am I from pointing at the foe?
const offset = angleDiff(foe.angleTo, self.angle);
```

#### `rand(min, max)`
Returns a random float between `min` and `max`.

```javascript
const jitter = rand(-0.1, 0.1);
```

---

## Sandboxing & safety

- Scripts run inside `new Function()` — they have **no access** to the DOM, `localStorage`, `canvas`, or any global game variables
- Each script is wrapped in `try/catch` — a crash only affects that one ship for that frame, the game continues normally
- Scripts are **time-limited to 2ms** — if your script runs too long it is killed and the ship falls back to normal AI
- The error message is shown both in the editor (red text below the textarea) and as `err` on the ship in Debug mode

---

## Presets

Five ready-made scripts are available in the Preset dropdown:

### Default AI
Mimics the built-in AI — faces the nearest enemy, fires when aligned, uses boost to close or retreat.

### Aggressive
Steers directly at the nearest enemy at all times. Fires whenever pointing close enough. Uses boost to close distance.

```javascript
if (!foe) return { steer: 0, shoot: false, boost: false };
var s = angleDiff(foe.angleTo, self.angle);
var shooting = Math.abs(s) < 0.3;
var b = false;
if (self.boostReady && foe.dist > 200) b = 'forward';
return { steer: s, shoot: shooting, boost: b };
```

### Kiter
Maintains an ideal distance of 180px from the nearest enemy. Retreats when too close, advances when too far. Boosts back when the enemy gets very close.

### Flanker
Approaches the enemy from a 90° angle instead of head-on. More evasive and harder to hit.

### Coward
Always runs away. Only fires when the enemy is behind it (within 30° of the ship's back).

---

## Writing your own Brain

A minimal working script:

```javascript
// Always chase and shoot nearest enemy
if (!foe) return { steer: 0, shoot: false, boost: false };

var steer = angleDiff(foe.angleTo, self.angle);
var shoot = Math.abs(steer) < 0.3 && self.cd === 0;

return { steer: steer, shoot: shoot, boost: false };
```

Tips:
- Always check `if (!foe)` first — there may be no enemies alive
- `angleDiff(foe.angleTo, self.angle)` is the most useful single expression — it tells you exactly how much to turn
- Keep scripts simple — complex logic may hit the 2ms timeout
- Use `var` instead of `let`/`const` for compatibility in older browsers
- The `steer` value is added to the ship's current heading each frame — small values (0.05–0.15) produce smooth turns

---

## Debug Mode

The Debug mode is a development tool that helps you observe and tune your Brain scripts in real time.

**How to activate:** Debug mode is only available when at least one Brain is active. The `⊕ Debug` button appears in the HUD next to the Pause button. Click it to toggle on/off.

**What it shows** (only for Brain-controlled ships):

| Visual | Description |
|---|---|
| Blue/green line | Current steering direction of the ship |
| Dashed red line | Line from the ship to its current target enemy |
| Small dot | Marks the targeted enemy |
| Yellow arrow | Boost direction when a boost is triggered |
| `err` text | Script crashed this frame |

> **Note:** Ships using the built-in AI show nothing in Debug mode — only Brain-controlled ships are visualised.

**Speed control** — when Debug mode is on, a speed slider appears in the HUD:

| Setting | Effect |
|---|---|
| `1.0x` | Normal speed |
| `0.75x` | Slightly slowed |
| `0.5x` | Half speed |
| `0.25x` | Quarter speed |
| `0.1x` | Very slow — almost frame-by-frame |
| `0x` | Frozen — step controls appear |

**Step mode (Speed = 0x):**

When frozen, four step buttons appear:

`◀◀ -10` — jump 10 frames back  
`◀ -1` — one frame back  
`+1 ▶` — one frame forward  
`+10 ▶▶` — jump 10 frames forward

- Going **back** restores a previously recorded frame — nothing is recalculated
- Going **forward** while still in recorded history replays the exact same frames
- Going **forward** past the last recorded frame simulates a new tick and records it
- The frame counter `128/450` shows your current position. It turns **orange** when you are viewing the past.
- Up to 600 frames (~10 seconds) are stored in the buffer while Debug is active

---

## Example: laser-dodging script

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
