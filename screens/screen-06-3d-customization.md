# Screen 06 — 3D Customization (CORE FEATURE)
> Status: 🔄 In Analysis
> Estimated Effort: TBD (outsourced 3D developer — quoted separately)
> Back to [INDEX.md](INDEX.md)

---

## ⚡ DECISIONS SUMMARY (Quick Reference)

| # | Decision | Status | Resolution |
|---|---|---|---|
| 1 | Touch controls | ❓ Ask client | Drag = Rotate, Hold = Move (pan), Pinch = Zoom — must confirm what "Move" means |
| 2 | Options panel position | ✅ Confirmed | Bottom sheet on mobile (follow prototype), Side panel on web |
| 3 | Size change behavior | ✅ Confirmed | Model swap (separate GLB per size) — NOT real-time resize |
| 4 | Color change | ❓ Ask client | Preset palette recommended — confirm with client |
| 5 | Material change | ✅ Confirmed | Named mesh swap — 3D artist must set up GLB properly — ask client about 3D artist briefing |
| 6 | Add/Remove elements | ✅ Confirmed | Toggle switches (show/hide mesh) in bottom sheet |
| 7 | First-person walkthrough | ❓ Ask client | Recommend Option A (look around from fixed point) for Phase 1 |
| 8 | Header buttons | ❓ Ask client | Back + Undo + Redo + Unknown button + Golden Save — must confirm 4th button |
| 9 | Save button | ✅ Confirmed | Follow prototype — golden button in top right header |
| 10 | Performance targets | ✅ Confirmed | Goes in outsourced dev hiring brief — 60fps on iPhone 12 + Samsung Galaxy A54 |
| 11 | 3D platform/tech | ✅ Confirmed | React Three Fiber + Expo GL (mobile), Three.js (web) |

---

## What the Client Said
> "3D Customization — Core Feature. Booth appears centered on screen. Drag → Rotate. Pinch → Zoom. Drag → Move. Options panel: Size [3x3][4x4][5x8], Colors (real-time), Materials (Wood/Metal/Glass), Elements (Add/Remove panels, logos, lighting). All changes update in real-time. References: IKEA Place, Planner 5D, Homestyler."

---

## What This Screen Actually Is

This is NOT a design creation tool. It is a **3D configurator** — the client uploads pre-made professional designs, users customize within defined options.

Think of it like a car configurator on BMW's website:
- You don't design a car
- You pick a model, change color, change wheels, choose interior
- All within options the manufacturer provides

Same here:
- Client uploads a booth design as a 3D model (GLB file)
- User picks size, color, material, toggles elements
- All within options the client defines per design

---

## All Decisions

---

### Decision 1 — Touch Controls

**Prototype says:** Drag → Rotate, Pinch → Zoom, Drag → Move

**Problem identified:** Drag is used for both Rotate AND Move — two gestures cannot be the same action.

**Our recommendation:**
- One finger drag → Rotate (orbit around model)
- Press and hold + drag → Move (pan camera)
- Two finger pinch → Zoom in/out

**Open question — what does "Move" mean exactly?**

| Move type | What it means |
|---|---|
| Pan the camera | Model stays still, viewpoint slides left/right/up/down |
| Move the whole model | Booth physically slides on screen |
| Move individual elements | User grabs logo panel and repositions it inside booth |

Most likely = pan the camera (same as Google Maps pan behavior).
Full element repositioning would be CAD-tool level complexity — not expected here.

**Ask client:** "When you say Move — do you mean panning the camera view around the design, or physically repositioning elements inside it?"

---

### Decision 2 — Options Panel Position

**Decision: Bottom sheet on mobile. Side panel on web. ✅ Confirmed — following prototype.**

**Mobile (bottom sheet):**
- Panel slides up from bottom
- Partially overlays the 3D view
- Collapsible — user taps collapse arrow → full screen 3D view
- Standard pattern: IKEA Place, Planner 5D both use bottom sheet

**Web (side panel):**
- Panel fixed on right side
- 3D view takes remaining left portion of screen
- No prototype for web — this is our recommendation
- More screen space on desktop allows permanent side panel without blocking

---

### Decision 3 — Size Selection

**Prototype shows:** `[3x3]` `[4x4]` `[5x8]` as tappable buttons

**How it actually works — Model Swap (not real-time resize):**

Real-time resizing would require all proportions to scale correctly (walls stretch, logo stays same size, counter stays proportional) — extremely complex and prone to visual errors.

**Industry approach (how IKEA does it):**
```
Client uploads 3 separate pre-made GLB files:
  - booth_modern_a_3x3.glb
  - booth_modern_a_4x4.glb
  - booth_modern_a_5x8.glb

User taps [4x4] → app unloads current model → loads 4x4 model
```
Looks like resizing. Is actually model swapping. Much cleaner result.

**What this means for the client:**
- 1 design × 3 sizes = 3 GLB files
- 10 designs × 3 sizes = 30 GLB files
- Client's 3D artist must produce all size variations
- Must tell client this upfront — it affects their production workload

**Selected size is also passed to Screen 8 (Final Preview) for the summary.**

---

### Decision 4 — Color Change

**Status: ❓ Ask client — recommendation below.**

Two options:

| Option | Description | Pros | Cons |
|---|---|---|---|
| Preset palette | Client defines 8–12 approved colors per design | Professional results, brand controlled | Less freedom for user |
| Full color picker | User picks any color from spectrum | Maximum user freedom | User can pick ugly/unrealistic colors |

**Our recommendation: Preset palette.**

The client is a design company. They know which colors work for their designs. A full color picker risks users choosing colors that look bad in the final design — which reflects badly on the client's brand, not the user's choice.

Preset palette = controlled, professional, realistic results every time.

**Ask client:** "Will you provide a list of approved colors for each design? We recommend a preset palette of 8–12 colors to ensure quality results."

---

### Decision 5 — Material Change

**Prototype shows:** `Wood` / `Metal` / `Glass` buttons

**How it actually works — Named Mesh Texture Swap:**

A 3D model is made of individual named pieces (meshes). Think of it like Lego bricks, each with a name.

```
booth_3x3.glb contains:
  ├── mesh: "wall_back"       ← tagged as "primary_surface"
  ├── mesh: "wall_left"       ← tagged as "primary_surface"
  ├── mesh: "wall_right"      ← tagged as "primary_surface"
  ├── mesh: "counter"         ← tagged as "primary_surface"
  ├── mesh: "logo_panel"
  ├── mesh: "ceiling_light"
  └── mesh: "floor"
```

When user taps `Metal`:
- Our code finds all meshes tagged as "primary_surface"
- Swaps their texture from wood → metal texture
- Happens in real-time in Three.js

**Critical requirement:** The client's 3D artist MUST:
1. Name and tag all meshes that are affected by material changes
2. Provide texture files for each material (wood, metal, glass) per design
3. This briefing to their 3D team is the client's responsibility

**Ask client:** "Are your 3D models already set up with named material zones? Or does your 3D artist need a technical brief from our developer?"

---

### Decision 6 — Add / Remove Elements

**Prototype says:** Add / Remove panels, logos, lighting

**How it actually works — Mesh Visibility Toggle:**

Each removable element (logo panel, lighting fixture, counter, side panel) is a separately named mesh inside the GLB file. It always exists in the file — we just show or hide it.

```
User taps: ☑ Logo Panel (toggle ON)
→ mesh "logo_panel" → visible = true

User taps: ☐ Ceiling Light (toggle OFF)
→ mesh "ceiling_light" → visible = false
```

**UI: Toggle switches in bottom sheet panel.**
- Simple, clear, user understands on/off state
- List of available elements defined per design
- Each design can have different elements available

**Same critical requirement as materials:** 3D artist must name each toggleable element as a separate mesh in the GLB file.

---

### Decision 7 — First-Person Walkthrough

**Prototype says:** 360° navigation inside the design (first-person view)

**Status: ❓ To be discussed further — recommendation below.**

Two implementation options:

| Option | Description | Complexity |
|---|---|---|
| **A — Look around (Phase 1)** | Camera moves to a fixed point inside the model. User drags to look in all directions. Like a 360° photo. No walking. | Medium |
| **B — Full walkthrough (Phase 2)** | User can actually walk through the space using on-screen joystick. Collision detection required (camera can't walk through walls). | Very High |

**Our recommendation: Option A for Phase 1.**
- Still impressive and immersive — client's users can stand inside the booth and look around
- Much less risk of bugs and performance issues
- Option B can be scoped as Phase 2 if client wants full navigation

**UI:** Dedicated "Walk Inside" button (or icon). Tapping it switches camera to first-person mode. "Exit" button or back gesture returns to orbit view.

**Ask client:** "For the first-person view — do you want users to just look around from inside (360° look), or actually walk/move through the space? We recommend look-around for Phase 1."

---

### Decision 8 — Screen Header

**Prototype shows (left to right):**
- ← Back arrow
- ↩ Undo button
- ↪ Redo button
- ❓ Unknown 4th button (to confirm with client)
- 💛 Golden "Save" button

**Our position:** Follow prototype exactly. No additions.

**4th button likely candidates based on similar apps:**
- Reset — returns design to original default state
- Screenshot/Snapshot — captures current view as image
- Fullscreen — hides header/panel for full 3D view

**Ask client:** "In the prototype header, there are 4 buttons before the Save button. What is the 4th button (the one after Undo and Redo)?"

---

### Decision 9 — Save Design Button

**Decision: Follow prototype. ✅ Confirmed.**

- Golden "Save" button in top right of header
- Tapping it → navigates to Screen 7 (Save Design confirmation)
- Saves all current customization state:
  - Selected size
  - Selected colors
  - Selected material
  - Active/inactive elements
  - Camera position (optional)

---

### Decision 10 — Performance Requirements

**This is a technical constraint — goes directly into the outsourced developer's hiring brief.**

| Requirement | Target |
|---|---|
| Max polygon count per model | Under 100,000 polygons |
| Texture size | Max 2048×2048px, compressed (KTX2 format) |
| Total GLB file size | Under 15MB per model |
| Target frame rate | 60fps minimum |
| Test devices minimum | iPhone 12 + Samsung Galaxy A54 |

**The client's 3D artist must also be briefed on these limits.** A beautiful 3D model in 3ds Max can be unusable on mobile if not optimized for web. The 3D artist needs to export specifically for mobile web — this is a specialist skill.

**Important:** When hiring the outsourced 3D developer, they must demonstrate they have done mobile 3D optimization before. Add to job post.

---

### Decision 11 — 3D Technology Stack

**Decision: React Three Fiber + Expo GL (mobile), Three.js (web). ✅ Confirmed.**

| Platform | Technology |
|---|---|
| Web (Next.js) | Three.js directly in browser — standard approach |
| iOS + Android (React Native) | React Three Fiber + Expo GL |

**Why not WebView (previous recommendation, now updated):**

WebView approach puts a full browser inside the app. Extra layer = extra memory, extra latency, slightly laggy controls on mid-range Android.

**React Three Fiber + Expo GL:**
- Expo GL gives direct GPU access — no browser layer in between
- React Three Fiber is Three.js written as React components
- 3D developer who knows Three.js can learn R3F quickly — same concepts
- Much better performance on mobile
- Runs natively on iOS and Android GPU

**For the outsourced developer job post:** Must know Three.js (for web) AND React Three Fiber + Expo GL (for mobile). These are related skills — most Three.js devs can learn R3F. Specify both in the job post.

---

## How 3D Customization Works End-to-End

```
Screen 5 (Loading) pre-loads the GLB model
         ↓
Screen 6 opens — model already in memory, instant render
         ↓
User sees booth centered on screen in orbit mode
         ↓
User interacts:
  • Drag to rotate / Hold+drag to pan / Pinch to zoom
  • Taps size [4x4] → unloads current GLB, loads 4x4 GLB
  • Taps color (preset) → Three.js swaps material color on mesh
  • Taps material [Metal] → Three.js swaps texture on tagged meshes
  • Toggles element [Logo Panel OFF] → mesh.visible = false
  • Taps [Walk Inside] → camera enters first-person mode
         ↓
User taps golden Save button
         ↓
App saves customization state object:
  {
    designId, selectedSize, selectedColor,
    selectedMaterial, activeElements[], cameraMode
  }
         ↓
Navigate to Screen 7 (Save confirmation)
```

---

## What Client Must Provide (Critical)

| Item | Who Provides | Why |
|---|---|---|
| GLB file per size per design | Client's 3D artist | One file per size variation |
| Named and tagged meshes in GLB | Client's 3D artist | Required for material + element changes to work |
| Texture files per material (wood/metal/glass) | Client's 3D artist | Required for material swap |
| Approved color palette per design | Client | Required for preset color picker |
| List of toggle-able elements per design | Client | Required to build the elements panel |

**None of the 3D customization features work without these assets being properly prepared.** This must be communicated to client before development begins.

---

## UI Layout (Mobile)

```
┌─────────────────────────────────────┐
│  ←   ↩  ↪  [?]          [💛 Save]  │  ← header
├─────────────────────────────────────┤
│                                     │
│                                     │
│         [ 3D MODEL ]                │
│      (booth rendered here)          │
│                                     │
│                   [👣 Walk Inside]  │  ← first-person toggle
│                                     │
├─────────────────────────────────────┤
│  ▲  Customize  ▲                    │  ← bottom sheet handle
│                                     │
│  SIZE:  [3x3]  [4x4✓]  [5x8]       │
│                                     │
│  COLOR: ● ● ● ● ● ● ● ●            │  ← preset swatches
│                                     │
│  MATERIAL: [Wood✓] [Metal] [Glass]  │
│                                     │
│  ELEMENTS:                          │
│    Logo Panel          ✓ ──────○    │
│    Ceiling Light       ✗ ○──────    │
│    Side Counter        ✓ ──────○    │
└─────────────────────────────────────┘
```

---

## Open Questions for Client (Screen 6)

- [ ] When you say "Move" in the controls — pan the camera, or reposition elements?
- [ ] Color change — preset palette (recommended) or full color picker?
- [ ] Will you provide approved colors per design, or should we allow any color?
- [ ] Are your 3D models already set up with named material zones? Does your 3D artist need a technical brief?
- [ ] First-person view — look around from fixed point, or full walk-through navigation?
- [ ] What is the 4th button in the header (after Undo and Redo)?
- [ ] How many size variations per design? (e.g. always 3 sizes, or varies per design?)
- [ ] What elements can be added/removed? (Provide list per design category)

---

## Outsourced Developer Hiring Brief (Key Points)

When posting the job for the 3D developer, they must:
- Know Three.js (for web implementation)
- Know React Three Fiber + Expo GL (for React Native mobile)
- Show portfolio of: 3D configurator with color/material change
- Show portfolio of: mobile-optimized 3D (under 15MB, 60fps on mid-range Android)
- Understand GLB/GLTF format, named mesh selection, texture swapping
- Implement first-person camera controls (at minimum look-around mode)

---

## Estimated Effort

> This screen is built entirely by the outsourced 3D developer. Pricing depends on their rate and scope confirmed with client. Estimated ranges below for reference when quoting the 3D module.

| Task | Hours |
|---|---|
| Three.js / R3F setup + GLB model loading | 6–8 hrs |
| Orbit controls (rotate, zoom, pan) | 3–4 hrs |
| Size change (model swap) | 4–6 hrs |
| Color change (preset palette) | 4–5 hrs |
| Material swap (texture swap on named meshes) | 6–8 hrs |
| Element toggle (mesh visibility) | 4–5 hrs |
| Bottom sheet options panel (mobile) | 4–5 hrs |
| Side panel (web) | 3–4 hrs |
| First-person camera mode (Option A) | 6–10 hrs |
| Header: Undo / Redo / Save logic | 5–6 hrs |
| Save customization state → navigate to Screen 7 | 2–3 hrs |
| Performance optimization (mobile) | 8–12 hrs |
| React Native integration (R3F + Expo GL) | 6–8 hrs |
| **Total** | **~61–84 hrs** |

**Estimated cost (outsourced at $35–50/hr): $2,135 – $4,200**
**Your client quote for 3D module: 15,000 – 25,000 AED** (covers Screen 5, 6, 7, 8 + your management margin)
