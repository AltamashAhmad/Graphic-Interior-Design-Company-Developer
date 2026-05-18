# Screen 11 — Profile (My Designs & Orders)

> Status: ✅ Analyzed
> Estimated Effort: 16–22 hrs
> Back to [INDEX.md](INDEX.md)

---

## ⚡ DECISIONS SUMMARY (Quick Reference)

| # | Decision | Status | Resolution |
|---|---|---|---|
| 1 | What is in the Profile screen? | ✅ Confirmed | Two tabs: My Designs (saved) + My Orders (submitted) |
| 2 | Authentication | ✅ Confirmed | Supabase Auth — email/password + Google sign-in |
| 3 | Guest users | ✅ Confirmed | App is browseable without login. Login required only at save/checkout |
| 4 | My Designs tab | ✅ Confirmed | Shows saved design cards — tap to resume 3D configurator |
| 5 | My Orders tab | ✅ Confirmed | Shows order history with status badges |
| 6 | Order statuses | ✅ Confirmed | Enquiry Received → In Review → Confirmed → Completed |
| 7 | Delete saved design | ✅ Confirmed | Long press or swipe to delete a saved design |
| 8 | Profile info editable | ✅ Confirmed | Name, phone, email, company — editable |
| 9 | Logout | ✅ Confirmed | Settings or top-right menu |

---

## What This Screen Is

The Profile screen is the user's personal hub. It serves two purposes:

1. **My Designs** — all designs the user saved (without ordering). They can come back, continue customizing, and order whenever ready.
2. **My Orders** — all submitted enquiries and paid orders, with current status.

This is also the screen where the user manages their account (edit info, logout).

---

## Screen Layout (Mobile)

```
┌─────────────────────────────────────┐
│  Profile                    ⚙       │  ← header + settings icon
├─────────────────────────────────────┤
│                                     │
│  👤  Ahmed Al Mansouri              │
│      ahmed@company.ae               │
│      Al Mansouri Trading LLC        │
│      [ Edit Profile ]               │
│                                     │
├──────────────┬──────────────────────┤
│  My Designs  │  My Orders           │  ← tab bar
├──────────────┴──────────────────────┤
│                                     │
│  ── MY DESIGNS TAB ──────────────   │
│                                     │
│  ┌──────────────────────────────┐   │
│  │ [thumbnail]  My Booth —      │   │
│  │              Expo 2026       │   │
│  │              4×4 · Oak Wood  │   │
│  │              Customization   │   │
│  │              [ Resume → ]    │   │
│  └──────────────────────────────┘   │
│                                     │
│  ┌──────────────────────────────┐   │
│  │ [thumbnail]  Version 2 —     │   │
│  │              Metal Finish    │   │
│  │              3×3 · Metal     │   │
│  │              Premium         │   │
│  │              [ Resume → ]    │   │
│  └──────────────────────────────┘   │
│                                     │
│  ── MY ORDERS TAB ───────────────   │
│                                     │
│  ┌──────────────────────────────┐   │
│  │ [thumbnail]  Modern Booth    │   │
│  │              ORD-2026-00142  │   │
│  │              2,625 AED       │   │
│  │              🟡 In Review    │   │  ← status badge
│  └──────────────────────────────┘   │
│                                     │
│  ┌──────────────────────────────┐   │
│  │ [thumbnail]  Classic Rest.   │   │
│  │              ORD-2026-00098  │   │
│  │              1,260 AED       │   │
│  │              ✅ Completed    │   │
│  └──────────────────────────────┘   │
│                                     │
└─────────────────────────────────────┘
```

---

## All Decisions

---

### Decision 1 — Two Tabs: My Designs + My Orders

**My Designs:**
- Designs the user SAVED (without ordering)
- The user can resume customization at any time
- Tapping "Resume" opens Screen 6 (3D Customization) with their saved configuration pre-loaded
- User can delete a saved design (long press or swipe left → delete)

**My Orders:**
- All submitted enquiries and paid orders
- Each order shows: design name, order ID, total, and current status
- Tapping an order shows full order detail (a simple detail modal/sheet)
- Orders cannot be deleted — they are permanent records

---

### Decision 2 — Authentication (Supabase Auth)

**Login options:**
- Email + password
- Google sign-in (optional but strongly recommended — most users prefer it)

**When login is required:**
- Saving a design (Screen 7)
- Submitting checkout (Screen 9)
- Viewing Profile (Screen 11)

**Browsing without login:**
- All of Screens 1–6 (splash, home, categories, design list, design details, 3D configurator) are accessible without login
- This reduces friction — users can explore the full app before committing to an account
- Login prompt only appears when they try to save or order

---

### Decision 3 — Order Status Flow

Orders move through these statuses (updated by client from admin panel):

| Status | Badge color | Meaning |
|---|---|---|
| Enquiry Received | 🔵 Blue | Order submitted, awaiting review |
| In Review | 🟡 Yellow | Client's team is reviewing the order |
| Confirmed | 🟠 Orange | Order confirmed, production/delivery in progress |
| Completed | 🟢 Green | Order fulfilled |
| Cancelled | 🔴 Red | Order cancelled |

Status is updated manually by the client from the admin panel. When status changes, the app reflects the new status on the next data refresh.

**Push notification on status change** — recommended Phase 2 feature (alerts user when their order moves to "Confirmed" or "Completed").

---

### Decision 4 — Resume a Saved Design

Tapping "Resume" on a saved design card:
1. Fetches the saved configuration from Supabase
2. Navigates to Screen 5 (3D Loading) → loads the correct GLB model
3. Opens Screen 6 (3D Customization) with all saved settings pre-applied:
   - Selected size
   - Selected colors
   - Selected material
   - Active/inactive elements

This is one of the most useful features of the app. The user doesn't start from scratch — everything is exactly as they left it.

---

### Decision 5 — Edit Profile

Tapping "Edit Profile" opens an edit form with:
- Full Name
- Phone Number
- Email Address
- Company Name

Changes saved to Supabase user profile. Email change requires re-verification (Supabase handles this automatically).

---

### Decision 6 — Settings & Logout

Tapping ⚙ (top right) opens a simple settings sheet:
- Edit Profile
- Language (English / Arabic — if client enables Arabic)
- Logout
- App version (bottom, small text)

---

## Web Layout (Desktop)

```
┌─────────────────────────────────────────────────────────────┐
│  Header nav: Logo | Home | Categories | Profile | Logout    │
├──────────────────┬──────────────────────────────────────────┤
│                  │                                           │
│  👤 Ahmed        │  [ My Designs ]  [ My Orders ]           │
│  ahmed@co.ae     │                                          │
│  Al Mansouri LLC │  ┌──────────┐ ┌──────────┐ ┌──────────┐ │
│                  │  │ Design 1 │ │ Design 2 │ │ Design 3 │ │  ← grid
│  [ Edit Profile ]│  │ Resume→  │ │ Resume→  │ │ Resume→  │ │
│                  │  └──────────┘ └──────────┘ └──────────┘ │
│  ─────────────── │                                          │
│  [ Logout ]      │                                           │
│                  │                                           │
└──────────────────┴──────────────────────────────────────────┘
```

---

## What Client Must Confirm

- [ ] Should Google sign-in be included at launch, or email/password only?
- [ ] What are the order status labels in Arabic (if Arabic is enabled)?
- [ ] Should users receive push notifications on order status change? (Phase 2?)
- [ ] Does the client want an admin panel notification when a new enquiry comes in? (Separate from WhatsApp — e.g. in-app badge on admin dashboard)

---

## Estimated Effort

| Task | Hours |
|---|---|
| Screen layout — mobile (tabs, design cards, order cards) | 3–4 hrs |
| Screen layout — web (two-column, design grid) | 2–3 hrs |
| Supabase Auth — email/password + Google sign-in | 3–4 hrs |
| Login gate (prompt when saving / at checkout) | 2 hrs |
| My Designs — fetch + display saved designs | 2–3 hrs |
| Resume design (load config → Screen 5 → Screen 6) | 3–4 hrs |
| My Orders — fetch + display orders with status badges | 2–3 hrs |
| Edit Profile (form + Supabase update) | 1–2 hrs |
| Delete saved design (long press / swipe) | 1 hr |
| Logout | 0.5 hrs |
| **Total** | **~19–28 hrs** |

**At 150 AED/hr: 2,850 – 4,200 AED**
