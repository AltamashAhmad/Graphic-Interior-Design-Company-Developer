# Screen 07 — Save Design

> Status: ✅ Analyzed
> Estimated Effort: 14–18 hrs
> Back to [INDEX.md](INDEX.md)

---

## ⚡ DECISIONS SUMMARY (Quick Reference)

| # | Decision | Status | Resolution |
|---|---|---|---|
| 1 | What does the Save button do? | ✅ Confirmed | Shows summary screen with two actions: Save to My Designs + Proceed to Checkout |
| 2 | Can user save for later? | ✅ Confirmed | Yes — "Save to My Designs" stores customization to profile |
| 3 | Package selection placement | ✅ Confirmed | On this screen — user picks Basic / Premium / Customization before proceeding |
| 4 | Summary card content | ✅ Confirmed | Design name, size, material, colors, active elements, package, price |
| 5 | Naming a saved design | ✅ Confirmed | Optional — user can name their saved version (e.g. "My Booth — Expo 2026") |
| 6 | Multiple saved versions | ✅ Confirmed | Yes — user can save multiple versions of the same design with different customizations |
| 7 | Data persisted | ✅ Confirmed | Full customization state saved: designId, size, colors, material, elements, package |

---

## What This Screen Is

The user has just finished customizing a 3D design on Screen 6 and tapped the golden Save button.

Screen 7 is a **review + action screen**. It does three things:
1. Shows a summary of everything the user configured
2. Lets the user pick their package (if not already chosen)
3. Gives two clear paths: save for later OR proceed to checkout

This is industry best practice for high-value B2B purchases. The user (a business buyer) may need to review the summary, share it internally, or get approval before purchasing. The "Save to My Designs" option removes friction — they can come back anytime and pick up where they left off.

---

## Screen Layout (Mobile)

```
┌─────────────────────────────────────┐
│  ←  Back          Your Design       │  ← header
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────────────────┐    │
│  │  [thumbnail of 3D design]   │    │  ← static snapshot of configured model
│  │                             │    │
│  │  Modern Exhibition Booth    │    │
│  │  ✏ Rename (optional)        │    │
│  └─────────────────────────────┘    │
│                                     │
│  ─────── CONFIGURATION ──────────   │
│  Size          4×4 m                │
│  Material      Oak Wood             │
│  Colors        Walls: White         │
│                Counter: Gold        │
│  Elements      Logo Panel  ✓        │
│                Ceiling Light  ✓     │
│                Side Counter  ✗      │
│                                     │
│  ─────── SELECT PACKAGE ─────────   │
│  ┌──────────┐ ┌──────────────────┐  │
│  │  Basic   │ │    Premium  ✓    │  │  ← tappable package cards
│  │  500 AED │ │   1,200 AED      │  │
│  └──────────┘ └──────────────────┘  │
│  ┌───────────────────────────────┐  │
│  │   Customization   2,500 AED   │  │
│  └───────────────────────────────┘  │
│                                     │
│  ┌───────────────────────────────┐  │
│  │    💛  PROCEED TO CHECKOUT    │  │  ← primary CTA (gold button)
│  └───────────────────────────────┘  │
│                                     │
│  [ 🔖 Save to My Designs ]          │  ← secondary action (outlined button)
│                                     │
└─────────────────────────────────────┘
```

---

## All Decisions

---

### Decision 1 — What the Save Button Triggers

**Tapping the golden Save on Screen 6 navigates to Screen 7.**

Screen 7 is NOT an auto-save. It is a deliberate review step before the user commits to either saving or purchasing.

This is intentional — for a B2B design app, users should always see a summary of what they configured before any action is taken.

---

### Decision 2 — Two Action Paths

**Path A — Proceed to Checkout (primary)**
- Gold button, prominent, full width
- Takes user to Screen 8 (Final Preview) → Screen 9 (Checkout)
- Requires package to be selected first — button disabled until package chosen
- This follows the prototype flow

**Path B — Save to My Designs (secondary)**
- Outlined/ghost button below the primary CTA
- Saves the full customization state to the user's profile
- User can come back to it anytime from Screen 11 (Profile)
- Does NOT require purchase
- Confirmation toast: "Design saved to your profile ✓"

**Why both:**
- Primary user goal = checkout (prototype confirms this)
- Secondary need = save for later (B2B reality — buyer ≠ decision maker always)
- Cost to build both = ~2 extra hours. Worth it every time.

---

### Decision 3 — Package Selection on This Screen

**The three packages are shown as selectable cards on Screen 7.**

| Package | What it includes | Price (example) |
|---|---|---|
| Basic | Product photos + description | ~500 AED |
| Premium | Basic + PDF drawings + 3ds Max file | ~1,200 AED |
| Customization | Premium + 3D interactive model | ~2,500 AED |

**UX rules:**
- If user arrived from Screen 6 (3D customization), the Customization package is pre-selected
- If user tapped a non-3D design, Customization package may be greyed out / not available
- Package prices are fetched dynamically from the database — client manages them from admin panel
- "Proceed to Checkout" button is disabled (greyed out) until a package is selected

---

### Decision 4 — Configuration Summary Card

Everything the user configured on Screen 6 is displayed here as a read-only summary:

| Field | Source |
|---|---|
| Design name | From database |
| Thumbnail image | Static snapshot / hero photo of design |
| Selected size | From Screen 6 state |
| Selected material | From Screen 6 state |
| Selected colors (per part) | From Screen 6 state |
| Active elements | From Screen 6 state (list of ON items only) |
| Selected package | Selected on this screen |
| Package price | From database |

**The user cannot edit from this screen** — if they want to change something, they tap Back to return to Screen 6. All their changes are preserved.

---

### Decision 5 — Rename the Saved Design

**Optional field — tapping ✏ lets user type a custom name.**

- Default name: the design's original name (e.g. "Modern Exhibition Booth")
- User can rename to: "My Booth — Expo 2026" or "Ahmed's Restaurant Design"
- This name appears in Screen 11 (Profile / My Designs)
- If user skips naming → saved with default design name + date stamp

This adds almost no complexity but makes the Profile screen far more useful — especially for users who save multiple versions of the same design.

---

### Decision 6 — Multiple Saved Versions

**A user can save multiple versions of the same base design.**

Example: The same "Modern Booth" design saved as:
- "Version 1 — Wood + 3×3" 
- "Version 2 — Metal + 4×4"
- "Version 3 — Gold scheme for Dubai Expo"

Each saved version stores a separate customization state object. This is simple to implement in the database — one row per saved design per user.

---

### Decision 7 — Data Saved to Database

When user taps "Save to My Designs", the following object is written to Supabase:

```json
{
  "userId": "uuid",
  "designId": "uuid",
  "customName": "My Booth — Expo 2026",
  "savedAt": "2026-05-16T10:00:00Z",
  "configuration": {
    "size": "4x4",
    "material": "oak_wood",
    "colors": {
      "walls": "#FFFFFF",
      "counter": "#C9A84C"
    },
    "activeElements": ["logo_panel", "ceiling_light"],
    "package": "customization"
  }
}
```

When user taps "Proceed to Checkout", the same object is passed to Screen 8 (Final Preview) and Screen 9 (Checkout) as the order payload.

---

## Screen Flow

```
Screen 6 (3D Customization)
  User taps golden Save button
         ↓
Screen 7 (Save Design)
  ├── User reviews summary
  ├── User selects package (if not pre-selected)
  │
  ├── Path A: Taps "Proceed to Checkout"
  │     ↓
  │   Screen 8 (Final Preview)
  │     ↓
  │   Screen 9 (Checkout)
  │
  └── Path B: Taps "Save to My Designs"
        ↓
      Toast confirmation: "Saved ✓"
        ↓
      User stays on Screen 7 (can still proceed to checkout)
      OR navigates to Screen 11 (Profile) to see saved designs
```

---

## Web Layout (Desktop)

```
┌─────────────────────────────────────────────────────────────┐
│  ← Back to Design                        [User avatar]      │
├───────────────────────┬─────────────────────────────────────┤
│                       │                                      │
│  [3D thumbnail]       │  Modern Exhibition Booth  ✏         │
│                       │                                      │
│                       │  SIZE        4×4 m                  │
│                       │  MATERIAL    Oak Wood                │
│                       │  COLORS      Walls: White            │
│                       │              Counter: Gold           │
│                       │  ELEMENTS    Logo Panel ✓            │
│                       │              Ceiling Light ✓         │
│                       │                                      │
│                       │  ── SELECT PACKAGE ──────────────   │
│                       │  [ Basic 500 ] [Premium 1200] [Customization 2500✓] │
│                       │                                      │
│                       │  [ 💛  PROCEED TO CHECKOUT → ]      │
│                       │  [ 🔖  Save to My Designs    ]      │
│                       │                                      │
└───────────────────────┴─────────────────────────────────────┘
```

---

## What Client Must Confirm

- [ ] Package names and prices — exact names and AED prices per design category
- [ ] Is package selection always on this screen, or sometimes on Screen 4 (Design Details)?
- [ ] Does the Customization package require the 3D model? (i.e. only available for designs that have a GLB file)

---

## Estimated Effort

| Task | Hours |
|---|---|
| Screen layout — mobile (summary card + package cards + CTAs) | 3–4 hrs |
| Screen layout — web (two-column layout) | 2–3 hrs |
| Package selection logic (pre-select if from 3D, disabled if no 3D model) | 2–3 hrs |
| Save to My Designs (write to Supabase + toast confirmation) | 2–3 hrs |
| Rename design (inline edit field) | 1–2 hrs |
| Pass configuration state to Screen 8 / 9 (checkout payload) | 2 hrs |
| Disabled CTA until package selected | 1 hr |
| **Total** | **~13–18 hrs** |

**At 150 AED/hr: 1,950 – 2,700 AED**
