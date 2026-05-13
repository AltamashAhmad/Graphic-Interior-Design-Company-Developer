# Screen 05 — 3D Loading Screen
> Status: ✅ Analyzed
> Estimated Effort: 6–8 hours
> **Quoted Price: 900–1,200 AED** (Web + iOS + Android)
> Back to [INDEX.md](INDEX.md)

---

## ⚡ DECISIONS SUMMARY (Quick Reference)

| Decision | Final Resolution |
|---|---|
| **What triggers this screen** | Customization package only — Basic/Premium go straight to checkout. To confirm with client. |
| **Loading message** | Keep exactly: "Preparing your interactive design…" (from prototype, no change) |
| **What loads during this screen** | 3D model (GLB/GLTF) is fetched and pre-loaded before entering Screen 6 |
| **Animation** | Progress bar + percentage (e.g. 68%) — following prototype |
| **What if loading fails** | Recommendation: timeout → error state with Retry + Go Back buttons |
| **Minimum display time** | YES — 1.5 seconds minimum even if model loads fast |
| **Can user go back?** | YES — back arrow returns to Screen 4. Android hardware back also works. |
| **Who builds this** | Outsourced 3D developer — included in 3D module, not billed separately |
| **Platform difference** | Same screen on mobile and web, minor layout difference only |

---

## What the Client Said
> "Loading screen. Message: 'Preparing your interactive design…'"

Simple screen. Client defined the message. Our job is to make it feel polished and handle edge cases properly.

---

## What This Screen Actually Does (Technical)

This screen is NOT just visual fluff. It is doing real work:

```
User taps "Customize in 3D" on Screen 4
         ↓
Navigate to Screen 5 (3D Loading)
         ↓
In background:
  → Fetch GLB/GLTF model URL from API (for this specific design)
  → Download the GLB file (can be 5–30MB depending on model complexity)
  → Pre-load into Three.js scene (parse mesh, set up lights, camera)
         ↓
Model fully loaded and ready
         ↓
Auto-navigate to Screen 6 (3D Customization)
```

**Why this matters:** 3D model files are large. Loading them takes 2–10 seconds on a real mobile network. If you skip this screen and try to load directly inside Screen 6, the user sees a blank black screen with no feedback — terrible UX. This screen exists to fill that gap.

---

## All Decisions

### Decision 1 — What Triggers This Screen

User must have:
1. Selected "Customization" package on Screen 4
2. Tapped the primary CTA ("Customize in 3D" or "Enter 3D" — exact label TBD)

This screen is **only reachable via Customization package**. Basic and Premium go directly to checkout, not here.

---

### Decision 2 — The Loading Animation

**Decision: Progress bar with percentage — following prototype. ✅ Confirmed.**

- Shows exact percentage (e.g. 68%) so user knows how far along the download is
- User sees real progress — not a spinner with no information
- On web: fetch with `onprogress` event → real byte-level progress
- On mobile (React Native): `expo-file-system` downloadAsync → progress callback → updates bar

---

### Decision 3 — Loading Message

**Decision: Keep exactly as prototype. ✅ Confirmed.**

Message: *"Preparing your interactive design…"*

No changes. Client defined this text. We respect it.

---

### Decision 4 — Minimum Display Time

Even if the model loads in 0.5 seconds (fast wifi, small model), don't skip this screen instantly.

**Minimum hold: 1.5 seconds**

Why: Flashing past a loading screen looks broken. User taps a button and is suddenly in a 3D environment with no transition — jarring.

Always show this screen for at least 1.5 seconds, then navigate when loading is complete AND minimum time has passed.

```
Math.max(loadingComplete, minimumDisplayTime)
→ Navigate to Screen 6
```

---

### Decision 5 — Error Handling

What if the 3D model fails to load? (no internet, server error, corrupted file)

**Never leave user stuck on a loading screen forever.**

| Scenario | What Happens |
|---|---|
| No internet | After 10s timeout → show error state |
| Server error (404, 500) | After first failure → show error state |
| Slow network | Show real progress bar — user sees it moving |
| Very slow (>30s) | Show "This is taking longer than expected…" + Cancel option |

**Error state UI:**
- Icon (warning or broken model icon)
- Message: "Couldn't load the design. Please check your connection and try again."
- Two buttons: "Retry" and "Go Back"

---

### Decision 6 — Can User Go Back?

**YES — always provide a back button / escape route.**

Never trap the user on a loading screen. If they change their mind, tap back → return to Screen 4 (Design Details).

On mobile: hardware back button (Android) must also work.
On web: browser back button must work.

---

### Decision 7 — What Data Is Passed to Screen 6

When navigating from Screen 5 → Screen 6, pass:

```typescript
{
  designId: string,           // which design
  designName: string,         // display in Screen 6 header
  packageType: 'customization',
  loadedModel: THREE.Object3D, // pre-loaded 3D scene object
  availableSizes: string[],   // e.g. ['3x3', '4x4', '5x8']
  availableMaterials: string[], // e.g. ['Wood', 'Metal', 'Glass']
  availableColors: string[],  // hex codes or color names
  basePrice: number           // starting price for this design
}
```

This means Screen 6 opens **instantly** with the model already loaded — no second loading wait.

---

### Decision 8 — Platform Differences

| Platform | Notes |
|---|---|
| iOS (React Native) | Screen renders inside WebView (Three.js runs in WebView) — loading happens before WebView mounts |
| Android (React Native) | Same as iOS — identical implementation |
| Web (Next.js) | Three.js runs natively in browser — slightly simpler, no WebView needed |

**The 3D loading screen itself is the same UI on all platforms.** Only the underlying loading mechanism differs slightly.

---

## UI Layout

```
┌─────────────────────────────┐
│  ← (back arrow)             │
│                             │
│                             │
│        [Brand Logo]         │
│                             │
│  ████████████░░░░░░  68%    │  ← progress bar + percentage
│                             │
│  Preparing your             │
│  interactive design…        │
│                             │
│                             │
└─────────────────────────────┘
```

---

## Technical Implementation

### Mobile (React Native)
```
Libraries needed:
- expo-file-system          → download GLB with progress callback
- react-native-reanimated   → smooth progress bar animation
- @react-navigation/native  → back navigation handling

Flow:
1. Screen mounts → start GLB download
2. Download progress → update progress bar (0–100%)
3. Download complete → parse into Three.js (inside WebView)
4. Scene ready + minimum 1.5s passed → navigate to Screen 6
5. On error → show error state with retry
```

### Web (Next.js)
```
Libraries needed:
- three (Three.js)           → load and parse GLB
- GLTFLoader (from Three.js) → handles GLB/GLTF format natively
- fetch with onprogress      → real download progress

Flow:
1. Page mounts → fetch GLB with progress tracking
2. onprogress event → update progress bar
3. GLTFLoader parses model → scene object ready
4. Scene ready + minimum 1.5s passed → navigate to Screen 6
5. On error → show error state with retry
```

---

## File Structure
```
src/
  screens/
    ThreeDLoadingScreen.tsx     ← main screen component
  components/
    ProgressBar.tsx             ← reusable animated progress bar
    LoadingError.tsx            ← error state component
  services/
    modelLoader.ts              ← handles GLB fetch + Three.js parsing
  utils/
    minDisplayTime.ts           ← utility: Math.max(loadTime, minTime)
```

---

## Open Questions for Client

- [ ] Does Customization package trigger this screen only, or should Basic/Premium also have a transition? (Our recommendation: Customization only)
- [ ] What is the approximate file size of your 3D models? (Affects loading time and user experience on mobile)

---

## Estimated Effort

| Task | Hours |
|---|---|
| Loading screen UI (progress bar + message + back button) | 2–3 hrs |
| GLB fetch with real progress tracking (mobile) | 2–3 hrs |
| GLB fetch with real progress tracking (web) | 1–2 hrs |
| Error state UI + retry logic | 1 hr |
| Minimum display time logic | 0.5 hrs |
| Navigation: back to Screen 4, forward to Screen 6 | 0.5 hrs |
| **Total** | **7–10 hrs** |

**Estimated Price: 1,050 – 1,500 AED (Phase 2 — billed with 3D module)**

> Note: This screen is part of the 3D module. It will be built by the outsourced 3D developer alongside Screen 6. Price above is included in 3D module quote.
