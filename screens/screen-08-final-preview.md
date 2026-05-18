# Screen 08 — Final Preview

> Status: ✅ Analyzed
> Estimated Effort: 10–13 hrs
> Back to [INDEX.md](INDEX.md)

---

## ⚡ DECISIONS SUMMARY (Quick Reference)

| # | Decision | Status | Resolution |
|---|---|---|---|
| 1 | 3D model or static preview? | ✅ Confirmed | Static snapshot + order summary card — no interactive 3D |
| 2 | What this screen shows | ✅ Confirmed | Design thumbnail, full config summary, package, price breakdown, confirm CTA |
| 3 | Edit from here? | ✅ Confirmed | Yes — "Edit Design" back button returns to Screen 6 with state preserved |
| 4 | Price breakdown | ✅ Confirmed | Package price shown clearly, VAT shown separately |
| 5 | Contact/delivery info | ✅ Confirmed | NOT on this screen — that is Screen 9 (Checkout) |
| 6 | CTA action | ✅ Confirmed | "Confirm & Proceed to Payment" → Screen 9 (Checkout) |

---

## What This Screen Is

Screen 8 sits between **Screen 7 (Save Design)** and **Screen 9 (Checkout)**.

It is a **read-only order review screen**. The user sees exactly what they are about to order — design, configuration, package, and price — before entering payment details.

This is standard e-commerce best practice. It exists to:
- Prevent accidental orders (user can still go back and edit)
- Build trust (user sees the full breakdown before paying)
- Reduce chargebacks and disputes (user confirmed they saw the summary)

**The 3D model is NOT interactive here.** The heavy lifting was Screen 6. Screen 8 shows a static thumbnail (hero image or auto-generated snapshot of the configured design). Fast to load, clear to read.

---

## Screen Layout (Mobile)

```
┌─────────────────────────────────────┐
│  ←  Edit Design      Final Preview  │  ← header — back goes to Screen 6
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────────────────┐    │
│  │                             │    │
│  │   [Design thumbnail image]  │    │  ← hero photo / static snapshot
│  │                             │    │
│  └─────────────────────────────┘    │
│                                     │
│  Modern Exhibition Booth            │  ← design name
│  My Booth — Expo 2026    ✏          │  ← user's saved name (editable)
│                                     │
│  ─────── YOUR CONFIGURATION ──────  │
│  Size          4×4 m                │
│  Material      Oak Wood             │
│  Colors        Walls → White        │
│                Counter → Gold       │
│  Elements      Logo Panel  ✓        │
│                Ceiling Light  ✓     │
│                Side Counter  ✗      │
│                                     │
│  ─────── ORDER SUMMARY ───────────  │
│  Package       Customization        │
│  Package Price           2,500 AED  │
│  VAT (5%)                  125 AED  │
│  ─────────────────────────────────  │
│  Total                   2,625 AED  │
│                                     │
│  ┌───────────────────────────────┐  │
│  │  💛  CONFIRM & PROCEED TO     │  │
│  │       PAYMENT                 │  │  ← primary CTA (gold)
│  └───────────────────────────────┘  │
│                                     │
└─────────────────────────────────────┘
```

---

## All Decisions

---

### Decision 1 — Static Preview, Not Interactive 3D

**The 3D interaction is complete.** Screen 6 was where the user configured everything. Reloading an interactive 3D model on Screen 8 adds:
- Load time (even with caching)
- Memory usage
- Complexity for no extra user value

**What we show instead:**
- The design's hero photo (already loaded from the database)
- OR an auto-generated screenshot taken at the end of Screen 6 (possible with Three.js `renderer.domElement.toDataURL()`)

**Recommendation:** Use the design's existing hero photo for simplicity in Phase 1. Auto-generated snapshot from the 3D session can be a Phase 2 enhancement.

---

### Decision 2 — Full Configuration Summary

Everything configured in Screen 6 is shown read-only:

| Field | Value shown |
|---|---|
| Design name | From database |
| User's custom name | From Screen 7 (editable here too) |
| Size | e.g. 4×4 m |
| Material | e.g. Oak Wood |
| Colors | Listed per part: Walls → White, Counter → Gold |
| Active elements | Only ON elements shown (cleaner read) |
| Package | e.g. Customization |

---

### Decision 3 — Edit From Here

**Header shows: ← Edit Design**

Tapping it returns to Screen 6 (3D Customization) with the full state preserved. The user does not lose their configuration. This is critical — no user should feel trapped at the review stage.

**State preservation:** The configuration object (size, material, colors, elements) is kept in app state (Zustand store) throughout Screens 6 → 7 → 8. Nothing is written to the database until the user explicitly saves or completes checkout.

---

### Decision 4 — Price Breakdown with VAT

UAE VAT is 5%. All prices shown on Screen 8 must include a clear breakdown:

```
Package price:    2,500 AED
VAT (5%):           125 AED
─────────────────────────────
Total:            2,625 AED
```

**Prices are dynamic** — fetched from the database. Client updates prices from admin panel. VAT calculation happens automatically in the app.

**Important:** If the app handles online payment (card), the VAT-inclusive total is what gets charged. If inquiry-only, the breakdown is still shown so the user knows exactly what to expect.

---

### Decision 5 — No Contact/Delivery Fields Here

Screen 8 is **review only**. Contact details, delivery address, and any form fields belong to Screen 9 (Checkout). Keeping these separate prevents Screen 8 from becoming a long scroll and makes each screen's purpose clear.

---

### Decision 6 — Confirm CTA

**"Confirm & Proceed to Payment" (gold button)**

Tapping this navigates to Screen 9 (Checkout).

If the app uses inquiry-only (no online payment), the button text changes to:
**"Submit Enquiry"** → triggers a WhatsApp/email inquiry with the full order summary attached.

Both modes use the same Screen 8 — only the button label and Screen 9 behaviour changes.

---

## Screen Flow

```
Screen 7 (Save Design)
  User taps "Proceed to Checkout"
         ↓
Screen 8 (Final Preview)
  ├── User reviews everything
  ├── ← Edit Design → back to Screen 6 (state preserved)
  │
  └── Taps "Confirm & Proceed to Payment"
         ↓
       Screen 9 (Checkout)
```

---

## Web Layout (Desktop)

```
┌─────────────────────────────────────────────────────────────┐
│  ← Edit Design                           Final Preview      │
├───────────────────────┬─────────────────────────────────────┤
│                       │                                      │
│  [Design thumbnail]   │  Modern Exhibition Booth             │
│                       │  My Booth — Expo 2026  ✏             │
│                       │                                      │
│                       │  ── YOUR CONFIGURATION ──────────   │
│                       │  Size        4×4 m                  │
│                       │  Material    Oak Wood                │
│                       │  Colors      Walls → White           │
│                       │              Counter → Gold          │
│                       │  Elements    Logo Panel ✓            │
│                       │              Ceiling Light ✓         │
│                       │                                      │
│                       │  ── ORDER SUMMARY ───────────────   │
│                       │  Package       Customization         │
│                       │  Price         2,500 AED             │
│                       │  VAT (5%)        125 AED             │
│                       │  ─────────────────────────────────  │
│                       │  Total         2,625 AED             │
│                       │                                      │
│                       │  [ 💛  CONFIRM & PROCEED TO PAYMENT ]│
│                       │                                      │
└───────────────────────┴─────────────────────────────────────┘
```

---

## What Client Must Confirm

- [ ] Payment method — card (PayTabs) or inquiry only? Affects CTA label on this screen
- [ ] VAT — is 5% UAE VAT applied to all packages?
- [ ] Should the design thumbnail be the hero photo or a 3D snapshot? (recommendation: hero photo for Phase 1)

---

## Estimated Effort

| Task | Hours |
|---|---|
| Screen layout — mobile (thumbnail + config summary + price breakdown + CTA) | 3–4 hrs |
| Screen layout — web (two-column) | 2–3 hrs |
| Dynamic price + VAT calculation | 2 hrs |
| State preservation across Screens 6 → 7 → 8 (Zustand) | 2 hrs |
| Edit Design back navigation (with state preserved) | 1 hr |
| CTA → navigate to Screen 9 / trigger inquiry | 1–2 hrs |
| **Total** | **~11–14 hrs** |

**At 150 AED/hr: 1,650 – 2,100 AED**
