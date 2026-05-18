# Screen 09 — Checkout

> Status: ✅ Analyzed
> Estimated Effort: 12–18 hrs (inquiry) / 22–32 hrs (card payment)
> Back to [INDEX.md](INDEX.md)

---

## ⚡ DECISIONS SUMMARY (Quick Reference)

| # | Decision | Status | Resolution |
|---|---|---|---|
| 1 | Payment method | ❓ Ask client | Two paths — see below. Recommendation: inquiry for Phase 1 |
| 2 | Contact fields required | ✅ Confirmed | Name, phone, email, company name, notes (optional) |
| 3 | Order summary visible | ✅ Confirmed | Compact summary shown at top of checkout screen |
| 4 | WhatsApp integration | ✅ Confirmed (if inquiry) | Auto-populates WhatsApp message with full order details |
| 5 | Card payment gateway | ❓ Ask client | PayTabs recommended for UAE market (if card path chosen) |
| 6 | Guest checkout vs login | ✅ Confirmed | Login required — ties order to user's profile/My Designs |
| 7 | Order confirmation | ✅ Confirmed | Navigates to Screen 10 on success |

---

## What This Screen Is

Screen 9 is the final step before an order is placed. The user:
1. Sees a compact summary of what they're ordering
2. Fills in their contact/delivery details
3. Either pays by card OR submits an inquiry

**This is the most business-critical screen in the app.** Every decision here directly affects revenue.

---

## ⚠ The One Decision That Changes Everything

**Does the client want online card payment or inquiry-based ordering?**

| | Inquiry Only | Card Payment |
|---|---|---|
| How it works | User submits order details → client's team contacts them | User pays directly in the app |
| Build complexity | Low | High |
| Time to build | 12–18 hrs | 22–32 hrs |
| Cost | ~1,800–2,700 AED | ~3,300–4,800 AED |
| Payment gateway needed | No | Yes (PayTabs) |
| Refund/dispute handling | Manual (client handles) | Automated (gateway handles) |
| Best for | Phase 1 / launch | Phase 2 or if client wants immediate revenue |
| Risk | Zero payment risk | Requires gateway setup, testing, security |

**Recommendation: Inquiry for Phase 1.**

Reasons:
- These are high-value B2B orders (booth designs, restaurant interiors) — most such deals happen after a conversation, not a tap-to-pay moment
- Inquiry flow lets the client's sales team qualify the buyer, answer questions, and upsell
- Card payment can be added cleanly as Phase 2 — the screen structure is the same, only the CTA and backend differ
- No payment gateway setup, PCI compliance concerns, or refund logic needed at launch

---

## PATH A — Inquiry-Based Checkout (Recommended Phase 1)

### How It Works

```
User taps "Confirm & Proceed" on Screen 8
         ↓
Screen 9 shows compact order summary + contact form
         ↓
User fills: Name, Phone, Email, Company, Notes
         ↓
Taps "Submit Enquiry"
         ↓
App does TWO things simultaneously:
  1. Saves order record to Supabase (status = "enquiry")
  2. Opens WhatsApp with pre-filled message to client's number
         ↓
Navigate to Screen 10 (Order Confirmation)
```

### Screen Layout — Path A (Mobile)

```
┌─────────────────────────────────────┐
│  ←  Back              Checkout      │
├─────────────────────────────────────┤
│                                     │
│  ── YOUR ORDER ──────────────────   │
│  Modern Exhibition Booth            │
│  4×4 m  ·  Oak Wood  ·  Customization│
│  Total: 2,625 AED (incl. VAT)       │
│                                     │
│  ── YOUR DETAILS ────────────────   │
│                                     │
│  Full Name *                        │
│  ┌───────────────────────────────┐  │
│  │ Ahmed Al Mansouri             │  │
│  └───────────────────────────────┘  │
│                                     │
│  Phone Number *                     │
│  ┌───────────────────────────────┐  │
│  │ +971 50 123 4567              │  │
│  └───────────────────────────────┘  │
│                                     │
│  Email Address *                    │
│  ┌───────────────────────────────┐  │
│  │ ahmed@company.ae              │  │
│  └───────────────────────────────┘  │
│                                     │
│  Company Name                       │
│  ┌───────────────────────────────┐  │
│  │ Al Mansouri Trading LLC       │  │
│  └───────────────────────────────┘  │
│                                     │
│  Additional Notes                   │
│  ┌───────────────────────────────┐  │
│  │ Need delivery by June 15...   │  │
│  └───────────────────────────────┘  │
│                                     │
│  ┌───────────────────────────────┐  │
│  │  💛  SUBMIT ENQUIRY           │  │
│  └───────────────────────────────┘  │
│                                     │
│  Our team will contact you within   │
│  24 hours to confirm your order.    │
│                                     │
└─────────────────────────────────────┘
```

### WhatsApp Auto-Message (Pre-filled)

When user taps Submit, the app opens WhatsApp with the client's business number and this message pre-filled:

```
New Design Enquiry from App

Customer: Ahmed Al Mansouri
Phone: +971 50 123 4567
Email: ahmed@company.ae
Company: Al Mansouri Trading LLC

Design: Modern Exhibition Booth
Size: 4×4 m
Material: Oak Wood
Colors: Walls → White, Counter → Gold
Elements: Logo Panel ✓, Ceiling Light ✓
Package: Customization
Total: 2,625 AED (incl. 5% VAT)

Notes: Need delivery by June 15

Order ID: ORD-2026-00142
```

This is generated dynamically from the order state. Client gets full order details instantly on WhatsApp, no manual copy-pasting needed.

---

## PATH B — Card Payment (Phase 2 / If Client Requests)

### Payment Gateway: PayTabs

PayTabs is the recommended gateway for UAE:
- Accepts UAE cards (Visa, Mastercard, Mada)
- Supports AED natively
- Arabic language support
- Used widely in UAE e-commerce
- React Native SDK available

### How It Works

```
User taps "Confirm & Proceed" on Screen 8
         ↓
Screen 9 shows compact order summary + contact form
         ↓
User fills: Name, Phone, Email, Company
         ↓
Taps "Pay 2,625 AED"
         ↓
PayTabs payment sheet opens (card number, expiry, CVV)
         ↓
Payment processed → PayTabs webhook → Supabase order updated
         ↓
Navigate to Screen 10 (Order Confirmation)
```

### Screen Layout — Path B (Mobile)

Same as Path A but CTA changes:

```
│  ┌───────────────────────────────┐  │
│  │  💳  PAY  2,625 AED           │  │  ← PayTabs payment sheet opens
│  └───────────────────────────────┘  │
│                                     │
│  🔒 Secured by PayTabs              │
│  Visa  Mastercard  Mada             │
```

### Additional Dev Work (Path B vs Path A)

| Extra Task | Hours |
|---|---|
| PayTabs SDK integration (React Native + Next.js) | 4–6 hrs |
| Payment webhook handler (Supabase edge function) | 3–4 hrs |
| Order status updates (pending → paid → confirmed) | 2–3 hrs |
| Refund handling logic | 2–3 hrs |
| Security: server-side payment verification | 2 hrs |
| **Additional hours vs Path A** | **+13–18 hrs** |

---

## Decisions Common to Both Paths

### Login Required

The user must be logged in to reach the checkout screen. If they reach Screen 9 without being logged in:
- App shows a login/signup prompt
- After login, they are returned to Screen 9 (order state preserved)

**Why:** Orders must be tied to a user account so they appear in Screen 11 (My Designs / Order History) and the client's admin panel.

### Form Validation

| Field | Validation |
|---|---|
| Full Name | Required, min 2 characters |
| Phone | Required, UAE format (+971 or 05X) |
| Email | Required, valid email format |
| Company Name | Optional |
| Notes | Optional, max 500 characters |

All validation happens client-side first, then server-side before the order is submitted.

### Order Record in Supabase

Regardless of payment path, an order record is written:

```json
{
  "orderId": "ORD-2026-00142",
  "userId": "uuid",
  "designId": "uuid",
  "customName": "My Booth — Expo 2026",
  "configuration": {
    "size": "4x4",
    "material": "oak_wood",
    "colors": { "walls": "#FFFFFF", "counter": "#C9A84C" },
    "activeElements": ["logo_panel", "ceiling_light"],
    "package": "customization"
  },
  "pricing": {
    "packagePrice": 2500,
    "vat": 125,
    "total": 2625,
    "currency": "AED"
  },
  "customer": {
    "name": "Ahmed Al Mansouri",
    "phone": "+971501234567",
    "email": "ahmed@company.ae",
    "company": "Al Mansouri Trading LLC",
    "notes": "Need delivery by June 15"
  },
  "status": "enquiry",
  "createdAt": "2026-05-18T10:00:00Z"
}
```

---

## Screen Flow

```
Screen 8 (Final Preview)
  User taps "Confirm & Proceed to Payment"
         ↓
Screen 9 (Checkout)
  ├── User fills contact form
  │
  ├── Path A: Taps "Submit Enquiry"
  │     ↓
  │   Order saved to Supabase (status = enquiry)
  │   WhatsApp opens with pre-filled message
  │     ↓
  │   Screen 10 (Order Confirmation)
  │
  └── Path B: Taps "Pay 2,625 AED"
        ↓
      PayTabs payment sheet
        ↓
      Payment success → Supabase updated (status = paid)
        ↓
      Screen 10 (Order Confirmation)
```

---

## What Client Must Confirm

- [ ] **Inquiry or card payment?** This is the single most important decision for this screen
- [ ] Client's WhatsApp business number (for auto-message routing)
- [ ] Does the client want email notification as well as WhatsApp?
- [ ] If card payment: does the client have a PayTabs account, or do they need setup guidance?
- [ ] Are company name and notes optional or required?

---

## Estimated Effort

### Path A — Inquiry Only

| Task | Hours |
|---|---|
| Screen layout mobile + web | 3–4 hrs |
| Form fields + validation (client + server side) | 3–4 hrs |
| WhatsApp deep link with pre-filled message | 2 hrs |
| Order record saved to Supabase | 2–3 hrs |
| Login gate (redirect to login if not authenticated) | 1–2 hrs |
| Navigate to Screen 10 on success | 1 hr |
| **Total** | **~12–16 hrs** |

**At 150 AED/hr: 1,800 – 2,400 AED**

### Path B — Card Payment (additional on top of Path A)

| Extra Task | Hours |
|---|---|
| PayTabs SDK integration | 4–6 hrs |
| Payment webhook + order status updates | 3–4 hrs |
| Refund logic + error handling | 2–3 hrs |
| Server-side payment verification (security) | 2–3 hrs |
| **Total Path B** | **~23–32 hrs** |

**At 150 AED/hr: 3,450 – 4,800 AED**
