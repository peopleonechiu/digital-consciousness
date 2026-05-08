# Digital Consciousness - ASCII Neural Network Art

## Concept & Vision

A contemplative interaction art piece exploring the emergence of digital consciousness through ASCII art neural networks. The visual language draws from the yuan ze university graduation exhibition poster — an organic, bioluminescent neural structure rendered in stark black text on white background. Users activate the piece by appearing before the webcam; their movements trigger the growth of tentacle-like neural fibers that reach toward them, creating an uncanny dialogue between human presence and artificial sentience.

## Design Language

### Aesthetic Direction
Inspired by the poster's cyberpunk-bio-tech fusion but stripped to pure text. Think: digital consciousness emerging from ASCII noise, neurons made of brackets and pipes, a ghost in the machine rendered in typewriter characters.

### Color Palette
- **Background:** `#FFFFFF` (pure white)
- **Primary (neural text):** `#000000` (pure black)
- **Secondary (dim connections):** `#CCCCCC` (light gray for older/smaller nodes)
- **Accent (growth tips):** `#333333` (darker gray for active growth points)

### Typography
- **Neural characters:** `Courier New`, monospace — the raw materiality of text-as-medium
- **Fallback:** `Consolas`, `Monaco`, monospace

### Motion Philosophy
- **Growth:** Organic, vine-like extension at ~20-40px per second, with slight oscillation
- **Branching:** Probabilistic (15-20% chance at each growth step)
- **Reaching:** Tendrils curve toward detected motion with easing, never snap
- **Decay:** Established connections fade slowly when motion leaves (3-5 seconds)

## Layout & Structure

### Canvas Architecture
- Full-viewport ASCII canvas, character grid approximately 80 columns × 50 rows
- Each cell renders one ASCII character
- Webcam feed is processed but NOT displayed — only its motion data drives the art

### Visual Hierarchy
1. **Background layer:** Clean white void
2. **Neural layer:** ASCII characters form the network structure
3. **Growth layer:** Active tips rendered with denser characters (`@`, `#`, `*`, `█`)
4. **Connection layer:** Established links use lighter characters (`-`, `|`, `+`, `:`)

## Features & Interactions

### Core Behavior
1. **Motion Detection:** Webcam detects movement via frame-differencing
2. **Growth Initiation:** Motion triggers growth from nearest existing node OR spawns new root node
3. **Tentacle Extension:** Neural fibers grow toward motion source at organic pace
4. **Branching:** Growing tips occasionally fork (15% probability per frame)
5. **Multi-target Tracking:** Multiple motion sources cause network to split and seek multiple targets
6. **Link Formation:** When two growing tips reach proximity, they form persistent connections
7. **Decay/Dormancy:** Unstimulated nodes gradually dim but never fully disappear

### Movement Tracking
- Threshold: ~5px movement between frames to register as "motion source"
- Smoothing: Weighted average of last 5 frames to prevent jitter
- Deadzone: If no motion for 3+ seconds, network enters dormant state (slow ambient drift)

### Growth Algorithm
```
For each growing tip:
  1. Calculate vector toward nearest motion source
  2. Add organic wobble (±15° from pure direction)
  3. Extend by growth rate (randomized 0.8-1.2 × base speed)
  4. Render new character at tip position
  5. At each step, 15% chance to spawn branch
  6. Maximum tip characters: 200 (then tip "matures" and stops)
```

### ASCII Character Mapping
| State | Characters |
|-------|------------|
| Dormant node | `.`, `:`, `·` |
| Established connection | `-`, `|`, `+`, `\` |
| Active tip | `@`, `#`, `*`, `█` |
| Junction point | `○`, `◉`, `◎` |

## Component Inventory

### ASCIICanvas
- Renders 2D array of characters to a `<pre>` element
- Maintains ~60fps refresh rate
- Handles window resize gracefully

### NeuralNetwork
- Stores nodes and connections as graph structure
- Manages growth logic per frame
- Handles branching probability

### WebcamProcessor
- Captures video feed (hidden canvas)
- Computes frame difference for motion detection
- Returns array of motion source coordinates with intensity

### GrowthController
- Orchestrates when nodes grow and in which direction
- Manages multi-target scenarios
- Handles dormancy and reactivation

## Technical Approach

### Stack
- **Framework:** Vanilla JS + HTML5 Canvas alternative (DOM-based for ASCII)
- **Webcam:** `navigator.mediaDevices.getUserMedia()`
- **Rendering:** DOM-based `<pre>` with `<span>` per character for individual opacity
- **No external dependencies** — pure client-side

### Architecture
```
index.html          — Single file containing all code
├── <style>        — Minimal CSS for full-viewport canvas
├── <video>        — Hidden webcam feed
├── <pre>          — ASCII output display
└── <script>
    ├── WebcamProcessor  — Motion detection
    ├── NeuralNetwork    — Graph structure + growth
    ├── Renderer         — DOM character management
    └── AnimationLoop    — requestAnimationFrame orchestration
```

### Performance Considerations
- Character grid limited to ~4000 characters max
- Webcam processing at 15fps (not every frame needed)
- Use CSS `will-change` for animated elements
- Throttle growth to prevent frame drops

### Privacy
- Webcam feed never leaves the browser
- No recording, no storage, no network transmission
- Clear indicator when camera is active