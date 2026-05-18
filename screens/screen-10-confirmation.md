# Screen 10 — Order Confirmation

> Status: ✅ Analyzed
> Estimated Effort: 5–7 hrs
> Back to [INDEX.md](INDEX.md)

---

## ⚡ DECISIONS SUMMARY (Quick Reference)

| # | Decision | Status | Resolution |
|---|---|---|---|
| 1 | What this screen shows | ✅ Confirmed | Success state + order ID + summary + next actions |
| 2 | Order ID format | ✅ Confirmed | ORD-YYYY-NNNNN (e.g. ORD-2026-00142) — generated server-side |
| 3 | Next actions from here | ✅ Confirmed | View in My Designs + Back to Home |
| 4 | Email confirmation | ✅ Confirmed | Auto email sent to customer with order summary (Supabase email trigger) |
| 5 | WhatsApp (inquiry path) | ✅ Confirmed | Already opened on Screen 9 — no repeat action needed here |
| 6 | Confetti / animation | ✅ Confirmed | Yes — lightweight success animation (Lottie) |

---

## What This Screen Is

The final screen in the order flow. It appears immediately after:
- **Inquiry path:** User tapped "Submit Enquiry" on Screen 9
- **Card payment path:** PayTabs confirmed payment success

This screen does three things:
1. Confirms the action was successful (clear, celebratory)
2. Shows the order reference number
3. Gives the user clear next steps

It is a simple screen — but it must be done right. A weak confirmation screen leaves the user wondering "did it work?" A strong one closes the loop and builds trust.

---

## Screen Layout (Mobile)

```
┌─────────────────────────────────────┐
│                                     │
│                                     │
│         ✅  (Lottie animation)      │  ← animated checkmark / confetti
│                                     │
│     Enquiry Submitted!              │  ← or "Payment Successful!" for Path B
│                                     │
│  Thank you, Ahmed. Our team will    │
│  contact you within 24 hours to     │
│  confirm your order.                │
│                                     │
│  ─────── ORDER DETAILS ──────────   │
│                                     │
│  Order ID      ORD-2026-00142       │
│  Design        Modern Booth         │
│  Package       Customization        │
│  Total         2,625 AED            │
│  Date          18 May 2026          │
│                                     │
│  A confirmation has been sent to:   │
│  ahmed@company.ae                   │
│                                     │
│  ┌───────────────────────────────┐  │
│  │  📁  VIEW IN MY DESIGNS       │  │  ← primary action
│  └───────────────────────────────┘  │
│                                     │
│  [ ← Back to Home ]                 │  ← secondary
│                                     │
└─────────────────────────────────────┘
```

---

## All Decisions

---

### Decision 1 — Success Message Text

Text adapts based on payment path:

**Inquiry path:**
> "Enquiry Submitted! Thank you, [Name]. Our team will contact you within 24 hours to confirm your order."

**Card payment path:**
> "Payment Successful! Thank you, [Name]. Your order has been confirmed."

Both use the customer's first name (pulled from the order record) for a personal touch.

---

### Decision 2 — Order ID

Format: `ORD-YYYY-NNNNN`
- Generated server-side (Supabase database sequence)
- Shown prominently on this screen
- Used for all future reference (client's admin panel, customer queries, WhatsApp)
- Also included in the confirmation email

---

### Decision 3 — Next Actions

**Primary: "View in My Designs"**
- Navigates to Screen 11 (Profile → My Designs tab)
- User can see their saved and ordered designs in one place

**Secondary: "Back to Home"**
- Returns to Screen 2 (Home / Categories)
- Allows user to continue browsing

**No back navigation in header** — the user must not be able to navigate back to checkout from here (prevents double submissions).

---

### Decision 4 — Confirmation Email

Auto-triggered via Supabase when order record is created.

Email contains:
- Order ID
- Design name + configuration summary
- Package + price breakdown (with VAT)
- Client's contact details ("For questions, contact us at...")
- Estimated response time (inquiry: 24 hrs / payment: immediate)

Email is sent to the customer's email address provided in Screen 9. Simple transactional email — no HTML templates needed for Phase 1, plain text is fine.

---

### Decision 5 — Lottie Animation

A lightweight Lottie animation plays when the screen loads:
- Animated green checkmark (inquiry)
- Animated gold confetti burst (card payment)

Lottie files are tiny (under 50KB), play once, then stop. Used in thousands of production apps. No performance concern.

Free Lottie animations available at lottiefiles.com — client does not need to provide anything.

---

## Screen Flow

```
Screen 9 (Checkout)
  Order submitted successfully
         ↓
Screen 10 (Order Confirmation)
  ├── Taps "View in My Designs" → Screen 11 (Profile)
  └── Taps "Back to Home" → Screen 2 (Home)
```

---

## Estimated Effort

| Task | Hours |
|---|---|
| Screen layout — mobile + web | 2–3 hrs |
| Dynamic text (inquiry vs payment, customer name) | 1 hr |
| Lottie animation integration | 1 hr |
| Confirmation email trigger (Supabase) | 1–2 hrs |
| Navigation (block back, View in My Designs, Home) | 1 hr |
| **Total** | **~6–8 hrs** |

**At 150 AED/hr: 900 – 1,200 AED**
