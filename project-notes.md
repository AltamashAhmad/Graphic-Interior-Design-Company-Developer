# Design App — Project Notes
> Client: Graphic Design & Interior Design Company (UAE)
> Developer: Altamash Ahmad
> Last Updated: May 11, 2026

---

## Project Overview

A cross-platform app (iOS + Android + Web) for a UAE design company.
Users can browse, select, purchase, and **3D-customize** design concepts (booths, restaurants, offices, etc.)

**Core Feature:** Real-time 3D customization — user enters design, rotates/moves elements, changes colors/materials/sizes, saves and orders.

---

## Platforms Required
- iOS
- Android
- Website

---

## Tech Stack (Recommended)

| Layer | Technology |
|---|---|
| Mobile (iOS + Android) | React Native + Expo |
| Web | Next.js |
| 3D Engine | Three.js / Babylon.js (web) + WebView in React Native |
| Backend | Node.js + NestJS |
| Database | PostgreSQL |
| File Storage | AWS S3 / Cloudflare R2 |
| Payments | Stripe or PayTabs (UAE) |
| Auth | JWT + refresh tokens |

**Smart shortcut:** Build 3D viewer once in Three.js for web → embed in React Native via WebView. One codebase for 3D, three platforms.

---

## Categories (from client)
1. Restaurants
2. Cosmetics
3. Booths
4. Exhibitions
5. Offices & Workspaces
6. Seasonal Events
> Client is open to adding more

---

## Package Types Per Design
- ✅ Basic
- ✅ Premium
- ⭐ Customization (3D interactive)

---

## Premium Package Deliverables (per design)
- PDF file (technical drawings with dimensions)
- 3ds Max file (editable source)
- High-quality images
> Files accessible after purchase only

---

## 3D Customization Requirements
- Real-time interactive 3D environment
- Drag → Rotate
- Pinch → Zoom
- First-person walkthrough (360° navigation inside design)
- Change colors in real-time
- Change materials (Wood / Metal / Glass)
- Adjust sizes (e.g. 3x3, 4x4, 5x8)
- Add/Remove elements (panels, logos, lighting)
- Save design
- Proceed to checkout from saved design

### ⚠️ CRITICAL OPEN QUESTION — Must ask client:
> "Will you provide the 3D models (GLB/GLTF format) for each design, or do we need to create them from scratch?"
> This changes scope and price dramatically.

---

## Development Phases & Timeline

| Phase | Scope | Timeline |
|---|---|---|
| Phase 1 | Auth, user profile, categories, design listings, admin CMS | 3–4 weeks |
| Phase 2 | Premium package, file downloads, payment checkout | 3–4 weeks |
| Phase 3 | Basic 3D viewer (rotate/zoom/pan, basic color swap) | 4–6 weeks |
| Phase 4 | Advanced 3D (materials, sizes, save, first-person nav) | 6–10 weeks |
| Phase 5 | Polish, iOS App Store + Google Play + web deploy, testing | 3–4 weeks |
| **Total** | **Full scope** | **~5–6 months** |
| **MVP** | Phase 1 + 2 + basic 3D | **2.5–3 months** |

---

## Pricing (UAE Market)

| Scope | Range |
|---|---|
| Web only + basic 3D | 15,000–25,000 AED |
| Web + iOS + Android + basic 3D | 35,000–60,000 AED |
| Full scope as described | 60,000–120,000 AED |
| Phase 1 + 2 only (no 3D) as MVP | 10,000–18,000 AED |

**Strategy:** Quote Phase 1+2 as first contract (8,000–12,000 AED). Phase 3+ as separate contract once trust is built.

---

## Contract / Business Rules
- 50% upfront before writing any code
- Written scope document before starting
- Scope must clearly say "3D viewer with provided models" NOT "3D modeling from scratch"
- Define what happens if contract ends early

---

## Open Questions for Client (Master List)

### General
- [ ] What is the app name?
- [ ] Brand colors (primary + secondary)?
- [ ] Single theme or dark mode support?
- [ ] Will you provide 3D models (GLB/GLTF), or do we create them?
- [ ] First-time user flow — onboarding slides or straight to login?

### Screen 1 — Splash
- [ ] Logo file in SVG or PNG (transparent background)
- [ ] Animation preference — approve fade-in + scale (2 seconds)?
- [ ] Tagline on splash or logo only?

---

## Screen-by-Screen Analysis

---

### Screen 1 — Splash Screen ✅ ANALYZED

**What client said:** Logo → simple animation → auto redirect to Home

| Decision | Resolution |
|---|---|
| Background | Dark + brand gradient (pending brand colors) |
| Animation | Fade in + scale up simultaneously |
| Duration | 1.5s animation + 0.5s hold = 2s total |
| Logic | Check auth token silently during animation |
| Redirect (logged in) | → Home |
| Redirect (logged out) | → Login |
| First-time user | → Onboarding (if any) OR Login — ask client |
| App name/tagline | Pending client answer |
| Dark mode | Pending client answer |

**Packages needed (React Native):**
- `expo-splash-screen` — controls native splash hide timing
- `expo-linear-gradient` — background gradient
- `react-native-reanimated` — smooth animation
- `@react-navigation/native` — redirect logic

**File structure:**
```
src/
  screens/
    SplashScreen.tsx
  navigation/
    AppNavigator.tsx
  assets/
    logo.svg / logo.png
```

**Logic flow:**
```
App opens
  → Native splash (static, instant)
  → SplashScreen.tsx mounts
  → Animation starts (1.5s)
  → Simultaneously: check AsyncStorage for auth token
  → Animation ends → 0.5s hold
  → Token valid → Home
  → No token → Login
```

**Estimated effort:** 5–6 hours
**Blocker:** Need logo (SVG) + brand colors from client

---

### Screen 2 — Home (Categories) 🔲 TODO
### Screen 3 — Booths List (Design Grid) 🔲 TODO
### Screen 4 — Design Details 🔲 TODO
### Screen 5 — 3D Loading Screen 🔲 TODO
### Screen 6 — 3D Customization (CORE) 🔲 TODO
### Screen 7 — Save Design 🔲 TODO
### Screen 8 — Final Preview 🔲 TODO
### Screen 9 — Checkout 🔲 TODO
### Screen 10 — Order Confirmation 🔲 TODO
### Screen 11 — Profile (My Designs / Orders) 🔲 TODO

---

## Total Effort Tracker

| Screen | Estimated Hours | Status |
|---|---|---|
| Screen 1 — Splash | 5–6 hrs | Analyzed |
| Screen 2 — Home | TBD | Pending |
| Screen 3 — Booths List | TBD | Pending |
| Screen 4 — Design Details | TBD | Pending |
| Screen 5 — 3D Loading | TBD | Pending |
| Screen 6 — 3D Customization | TBD | Pending |
| Screen 7 — Save Design | TBD | Pending |
| Screen 8 — Final Preview | TBD | Pending |
| Screen 9 — Checkout | TBD | Pending |
| Screen 10 — Confirmation | TBD | Pending |
| Screen 11 — Profile | TBD | Pending |
| Backend + API + DB | TBD | Pending |
| Admin CMS | TBD | Pending |
| 3D Engine setup | TBD | Pending |
| **TOTAL** | **TBD** | |

---

## Notes & Decisions Log

- `May 11` — Client brief received via WhatsApp. Requirements clear and structured.
- `May 11` — Client has prototype flow (11 screens defined).
- `May 11` — Client is in a hurry — communicated 4–5 days needed for full scoping.
- `May 11` — 3D models question is the biggest unknown. Must resolve before quoting Phase 3+.
- `May 11` — Smart approach: quote Phase 1+2 first, build trust, then Phase 3 (3D).
- `May 11` — Screen 1 (Splash) fully analyzed. Waiting for logo + brand colors.
