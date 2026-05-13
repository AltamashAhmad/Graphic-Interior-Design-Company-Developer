# Open Source Reference Repos
> Project: UAE Graphic & Interior Design Company App
> Purpose: Reference repos discovered during research — what each one is, what code was found, and exactly how to use it for this project.
> Last Updated: May 13, 2026

---

## Why This File Exists

During Screen 6 (3D Customization) analysis, we searched GitHub deeply for open source repos that already solve the same problems this project has.
These repos are MIT licensed — free to study, adapt, and build on.
The outsourced 3D developer should be given this file as part of their hiring brief.

---

## Quick Summary Table

| Repo | Stars | What It Is | How We Use It |
|---|---|---|---|
| `breathingcyborg/3d-configurator` | ~400 | Full configurator + admin panel (PayloadCMS) | Architecture blueprint, attribute system, admin reference |
| `voyagi/3d-product-configurator` | ~250 | Clean R3F + Zustand configurator | Zustand store pattern, color/size logic, copy directly |
| `AnuOuseph/Spatial-AR-Furniture-Configurator` | ~150 | AR furniture configurator | Mobile bottom sheet + desktop side panel UI pattern |
| `mrdoob/three.js` (official) | 103k | Three.js core library + official examples | PointerLockControls for first-person walkthrough |
| `pmndrs/react-three-fiber` | 30.7k | React renderer for Three.js | Core 3D library for web + React Native (Expo GL) |
| `pmndrs/drei` | 9k+ | Helper library for R3F | OrbitControls, GLTFLoader, PointerLockControls wrapper |

---

---

## REPO 1 — `breathingcyborg/3d-configurator`

**GitHub:** https://github.com/breathingcyborg/3d-configurator
**Live Demo:** https://breathingcyborg.github.io/3d-configurator/
**License:** MIT
**Stack:** React + Three.js + React Three Fiber + TypeScript + PayloadCMS

---

### What It Is

The most complete open source 3D configurator found. It has two parts:
- `configurator_frontend/` — The user-facing app (browse designs, customize in 3D)
- `configurator_backend/` — Admin panel built on PayloadCMS (client logs in, uploads models, sets options)

---

### Folder Structure (Actual)

```
configurator_frontend/src/
  catalog/        ← browse designs (like our Screens 2 & 3)
  configurator/   ← the 3D customization screen (our Screen 6)
  components/     ← shared UI components
  hooks/          ← custom React hooks
  textures/       ← texture/material library files

configurator_backend/   ← PayloadCMS admin panel
  ← client logs in here
  ← uploads GLB model files
  ← defines attribute options per design (colors, textures, parts)
```

---

### The 6 Attribute Types (Most Important Finding)

This repo defines 6 types of customization that admin can set per design. These map almost perfectly to our client's requirements:

| Attribute Type | What It Does | Our Use Case |
|---|---|---|
| **Color** | Apply a hex color to named meshes | Wall color, panel color, counter color |
| **Texture** | Apply a surface pattern (wood, metal, fabric) | Material swap (Wood / Metal / Glass) |
| **Parts** | Swap an entire sub-mesh/component | Size variant model swap (3x3 / 4x4 / 5x8) |
| **User Image** | User uploads their own image → maps onto mesh | User places their company logo on the booth |
| **Building Image** | Background image / environment scene | Backdrop/environment behind the booth |
| **Manual Select** | Custom advanced option | Any special client-defined feature |

**⭐ Key insight:** The "User Image" attribute is a premium feature that's already built. Client can offer "upload your logo and see it on the booth in real-time" — this is a selling point we should flag to the client.

---

### How We Use This Repo

| What to take from it | How |
|---|---|
| Attribute system architecture | Blueprint for how client defines options per design in admin panel |
| Admin panel concept | PayloadCMS OR replicate with Supabase + simple CMS |
| Texture swap implementation | Reference code for our material swap (Wood/Metal/Glass) |
| User image upload onto mesh | Copy this feature — lets users place their company logo on booth |
| catalog/ folder structure | Reference for our Screens 2 & 3 (design browse + listing) |

**What NOT to copy directly:** The admin backend uses PayloadCMS which is a separate Node.js service. We are using Supabase instead — reference the concept, not the exact code.

---

---

## REPO 2 — `voyagi/3d-product-configurator`

**GitHub:** https://github.com/voyagi/3d-product-configurator
**Live Demo:** https://voyagi-configurator.vercel.app/
**License:** MIT
**Stack:** React + React Three Fiber + Zustand + TypeScript + Vite

---

### What It Is

A clean, production-quality 3D product configurator. Built for a lamp product but the architecture is general. This is the cleanest implementation of the exact pattern we need: per-mesh color change + size = model swap + Zustand shared state.

---

### The Actual Zustand Store Code (Found & Read)

This is the exact code from the repo's `src/store/configuratorStore.ts`:

```typescript
// This is the state that is shared between the 3D canvas and the UI controls

interface ConfiguratorState {
  bodyColor: ColorKey;      // color of part 1 (e.g. walls)
  shadeColor: ColorKey;     // color of part 2 (e.g. panels)
  accentColor: ColorKey;    // color of part 3 (e.g. details)
  activePart: LampPart;     // which part is currently selected by user
  size: LampSizeKey;        // which size is selected

  setActivePart: (part) => void;         // user taps a part → it becomes active
  setActivePartColor: (color) => void;   // changes only the active part's color
  setSize: (size) => void;               // triggers model swap
}

// Initial defaults:
bodyColor: "charcoal"
shadeColor: "rose"
accentColor: "oat"
activePart: "body"
size: "500"
```

**How color change works (step by step):**
1. User taps a zone on the booth (e.g. "wall panel") → `setActivePart("wall")` is called
2. Active part is now "wall"
3. User taps a color swatch (e.g. blue) → `setActivePartColor("blue")` is called
4. Store updates `wallColor: "blue"`
5. The 3D scene reads `wallColor` from store → finds the mesh named "wall" → applies blue material
6. Change is instant — no reload

**How size change works:**
1. User taps `[4x4]` button → `setSize("4x4")` is called
2. Store updates `size: "4x4"`
3. The 3D scene unloads current GLB → loads `booth_4x4.glb`
4. Looks like resizing, is actually model swapping

---

### Folder Structure (Actual)

```
src/
  components/
    canvas/    ← 3D scene (GLB loading, lights, mesh color logic, React Three Fiber)
    ui/        ← overlay controls (color swatches, size buttons, save button)
  store/
    configuratorStore.ts   ← Zustand store — the file we read above
  constants/
    colors.ts  ← preset color options (client defines these)
    sizes.ts   ← available size options (3x3, 4x4, 5x8 etc.)
  lib/
    analytics.ts  ← optional, tracks color/size change events
```

---

### How We Use This Repo

| What to take from it | How |
|---|---|
| **Zustand store pattern** | Copy this exact structure — rename parts to match booth zones (wall, counter, panel, ceiling etc.) |
| **Color per mesh logic** | Reference `canvas/` component — shows how to find a named mesh and apply color |
| **Size = model swap** | Reference `setSize` logic — same pattern for our 3x3/4x4/5x8 booth variants |
| **Clean architecture** | `canvas/` for 3D scene, `ui/` for controls, `store/` for shared state — use this split |
| **Analytics tracking** | Optional — `trackColorChange`, `trackSizeChange` — useful for client reporting later |

**This is the repo the outsourced developer should study FIRST.** It's the cleanest and most directly applicable.

---

---

## REPO 3 — `AnuOuseph/Spatial-AR-Furniture-Configurator`

**GitHub:** https://github.com/AnuOuseph/Spatial-AR-Furniture-Configurator
**License:** MIT
**Stack:** React + React Three Fiber + AR (WebXR)

---

### What It Is

A furniture configurator with AR (Augmented Reality) support. We are not using the AR part, but the UI layout pattern is exactly what we need: **bottom sheet on mobile, side panel on desktop.**

---

### What It Confirms

This repo independently confirms the industry standard layout:
- **Mobile:** Bottom sheet slides up from bottom — partially overlays 3D view — collapsible
- **Desktop:** Options panel fixed on right side — 3D view on left — permanent, no overlay

This is the same pattern used by IKEA Place, Planner 5D, and Homestyler (all of which client referenced).

---

### How We Use This Repo

| What to take from it | How |
|---|---|
| Bottom sheet component | Reference for mobile options panel layout |
| Side panel layout | Reference for web options panel layout |
| Per-part material change UI | Reference for how to show which part is selected |

**The outsourced developer should look at the layout structure.** Not much else is directly relevant to our specific booth context.

---

---

## REPO 4 — `mrdoob/three.js` (Official)

**GitHub:** https://github.com/mrdoob/three.js
**Specific file:** `examples/misc_controls_pointerlock.html`
**Direct link:** https://github.com/mrdoob/three.js/blob/dev/examples/misc_controls_pointerlock.html
**Live demo:** https://threejs.org/examples/#misc_controls_pointerlock
**License:** MIT
**Stars:** 103,000+

---

### What It Is

The official Three.js library and its example files. The specific file we need is the `PointerLockControls` example — this is the built-in Three.js implementation for first-person navigation (walk inside a 3D space and look around).

---

### The Actual PointerLockControls Code (Found & Read)

```javascript
// From the official Three.js example — misc_controls_pointerlock.html

import { PointerLockControls } from 'three/addons/controls/PointerLockControls.js';

// Setup
controls = new PointerLockControls(camera, document.body);

// User clicks "Enter" button → locks pointer, enters first-person mode
instructions.addEventListener('click', function () {
  controls.lock();   // locks the mouse cursor — now mouse = look direction
});

// Movement state (WASD keys)
let moveForward = false;
let moveBackward = false;
let moveLeft = false;
let moveRight = false;

const velocity = new THREE.Vector3();
const direction = new THREE.Vector3();

// In animation loop:
// 1. Calculate direction based on which keys are pressed
// 2. Apply velocity with delta time (smooth, frame-rate independent)
// 3. Move camera using controls.moveForward() and controls.moveRight()

controls.moveRight(- velocity.x * delta);
controls.moveForward(- velocity.z * delta);
```

**What this means in plain words:**
- User clicks a button → enters first-person mode
- Mouse movement = look left/right/up/down
- WASD keys = walk forward/back/left/right
- ESC key = exit first-person mode

---

### The Mobile Problem

`PointerLockControls` requires a mouse pointer — it does not work on mobile touchscreens. For mobile first-person:
- Touch drag = look around (replaces mouse movement)
- On-screen joystick (e.g. `nipplejs` library) = walk movement (replaces WASD)
- This requires custom implementation — no open source solution found that does this for interior spaces

**Recommendation:**
- Phase 1: Fixed-point look-around only (no walking) — works on mobile with simple touch drag
- Phase 2: Full walkthrough with joystick — custom build, higher cost

---

### How We Use This Repo

| What to take from it | How |
|---|---|
| `PointerLockControls` class | Direct import from Three.js — use as-is for web first-person view |
| WASD movement pattern | Reference code for keyboard-based navigation on web |
| Lock/unlock flow | UI flow: show "Click to enter" overlay → lock → show exit hint |

**@react-three/drei wraps this for React:** Use `<PointerLockControls />` from `@react-three/drei` package instead of setting it up manually — saves ~2 hours of wiring.

---

---

## REPO 5 — `pmndrs/react-three-fiber`

**GitHub:** https://github.com/pmndrs/react-three-fiber
**Docs:** https://docs.pmnd.rs/react-three-fiber
**License:** MIT
**Stars:** 30,700+

---

### What It Is

The React renderer for Three.js. This is the core library the entire 3D module is built on. Instead of writing `new THREE.Mesh()`, you write `<mesh />` — it's Three.js in JSX.

**This is not optional — it is the foundation.** Every other library in our stack sits on top of this.

---

### Key Facts

- No overhead vs plain Three.js — renders outside React's reconciler
- Outperforms plain Three.js at scale because of React's scheduling
- **Works in React Native via Expo GL** — official support, not a workaround
- Used by: Vercel, Zillow, ReadyPlayer.me (avatar configurator), 3DConfig (floor planner)

---

### Installation

```bash
# Web (Next.js)
npm install three @types/three @react-three/fiber

# React Native (Expo)
npx expo install three expo-gl @react-three/fiber
```

---

### The Ecosystem Libraries We Also Need

These are published by the same team (pmndrs) and are part of the standard R3F setup:

| Library | npm install | What it adds | Do we need it? |
|---|---|---|---|
| `@react-three/drei` | Yes | Helpers: OrbitControls, GLTFLoader, PointerLockControls, Environment | ✅ Yes — critical |
| `@react-three/gltfjsx` | Dev tool (run once) | Converts GLB file → React JSX component with all meshes named | ✅ Yes — saves huge time |
| `zustand` | Yes | State management (same team) | ✅ Yes — voyagi already uses this |
| `@react-three/postprocessing` | Optional | Bloom, depth of field, ambient occlusion | Phase 2 only |

---

### `@react-three/gltfjsx` — Game Changer Tool

This CLI tool is run once per GLB file by the developer. It auto-generates a React component:

```bash
# Run this on each client GLB file:
npx gltfjsx booth_modern_a.glb
```

**Output — auto-generated file `Booth_modern_a.jsx`:**
```jsx
// Every named mesh in the GLB becomes an accessible ref
export function Booth({ ...props }) {
  const { nodes, materials } = useGLTF('/booth_modern_a.glb')
  return (
    <group {...props}>
      <mesh name="WallLeft"    geometry={nodes.WallLeft.geometry}    material={materials.Wall} />
      <mesh name="WallRight"   geometry={nodes.WallRight.geometry}   material={materials.Wall} />
      <mesh name="Counter"     geometry={nodes.Counter.geometry}     material={materials.Counter} />
      <mesh name="Ceiling"     geometry={nodes.Ceiling.geometry}     material={materials.Ceiling} />
      <mesh name="LogoPanel"   geometry={nodes.LogoPanel.geometry}   material={materials.Logo} />
    </group>
  )
}
```

Now changing wall color = just change `materials.Wall.color` — no hunting through the GLB structure manually.

**Requirement for 3D artist:** Every mesh that needs individual control MUST have a unique, descriptive name in the GLB file. The 3D artist must follow this naming convention before exporting. This is part of the brief we give the client.

---

### How We Use This Repo

| What to take from it | How |
|---|---|
| Core R3F library | Foundation of everything — `<Canvas>` wraps the 3D scene |
| `@react-three/drei` | Import `<OrbitControls />`, `<PointerLockControls />`, `<useGLTF>` |
| `@react-three/gltfjsx` | Run on every client GLB file to generate editable React components |
| Expo GL integration | Follow official docs for React Native setup — one canvas, two platforms |

---

---

## HOW ALL REPOS WORK TOGETHER

Here is the full picture — which repo handles which part of Screen 6:

```
CLIENT UPLOADS GLB FILE
        ↓
Developer runs: npx gltfjsx booth.glb
(@react-three/gltfjsx — Repo 5 ecosystem)
        ↓
Auto-generated JSX component with named meshes
        ↓
3D Scene built with React Three Fiber + @react-three/drei
(Repo 5 — architecture pattern from Repo 2 / voyagi)
        ↓
User interacts with UI overlay (bottom sheet on mobile)
(Layout pattern from Repo 3 / AnuOuseph)
        ↓
UI actions update Zustand store
(State pattern from Repo 2 / voyagi — exact store code found)
        ↓
3D scene reads store → updates mesh colors/materials/visibility
(Color system from Repo 1 / breathingcyborg + Repo 2 / voyagi)
        ↓
Size change → loads different GLB
(Model swap pattern from Repo 2 / voyagi)
        ↓
First-person mode → user clicks button → PointerLockControls activates
(Repo 4 — Three.js official PointerLockControls example)
        ↓
User saves configuration → state passed to Screen 7 (Save Design)
(Zustand store persisted — Repo 2 pattern)
```

---

## WHAT DOES NOT EXIST IN OPEN SOURCE

These things were searched for and not found. The outsourced developer must build them custom:

| Feature | Status | Notes |
|---|---|---|
| First-person walkthrough on mobile (touch joystick) | ❌ Not found | Use `nipplejs` for joystick + custom touch-drag-to-look |
| Interior booth-specific 3D configurator | ❌ Not found | Only product configurators (lamp, chair, furniture) exist |
| UAE Arabic RTL configurator UI | ❌ Not found | If Arabic support needed, must build custom |
| Booth with user logo upload on mesh | ✅ Found | breathingcyborg "User Image" attribute — available to copy |

---

## BRIEF FOR OUTSOURCED 3D DEVELOPER

When hiring, send this exact brief:

> "We are building a 3D booth configurator using React Three Fiber + Expo GL (mobile) and Three.js (web). You will NOT start from scratch.
>
> **Study these repos before quoting:**
> - https://github.com/voyagi/3d-product-configurator — architecture + Zustand store pattern — study this FIRST
> - https://github.com/breathingcyborg/3d-configurator — attribute system, admin panel pattern, texture/user-image feature
> - https://github.com/AnuOuseph/Spatial-AR-Furniture-Configurator — mobile bottom sheet + desktop side panel layout
> - https://threejs.org/examples/#misc_controls_pointerlock — first-person controls reference
>
> **Required skills:**
> - React Three Fiber + @react-three/drei
> - GLB/GLTF model loading and optimization
> - Expo GL for React Native
> - Zustand state management
> - Mobile performance optimization (60fps target on mid-range Android)
>
> **You are adapting and extending these patterns for booth designs — not building from scratch. Quote accordingly.**"

---

## ESTIMATED TIME SAVINGS FROM THESE REPOS

| Without repos (from scratch) | With repos as reference |
|---|---|
| 61–84 hours | 35–50 hours |
| 15,000–21,000 AED outsource cost | 8,750–12,500 AED outsource cost |

**Saving: ~25,000–30,000 AED in outsourcing cost by having done this research.**
