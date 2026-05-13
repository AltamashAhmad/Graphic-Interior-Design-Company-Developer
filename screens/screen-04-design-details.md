# Screen 04 — Design Details
> Status: 🔄 IN ANALYSIS
> Last Updated: May 12, 2026

---

## What This Screen Does

When a user taps a design card from Screen 3 (Design List), they land here.
This is the **most important screen in the app** — it's where the user decides whether to buy, customize, or leave.

It shows:
- Images of the design (slider)
- Design name, description, size, material
- Package options (Basic / Premium / Customization)
- Download or customization CTA depending on package
- Favorite button

---

## DECISIONS SUMMARY

| # | Decision | Status | Resolution |
|---|---|---|---|
| 1 | Image Slider style | ✅ Decided | Dots on mobile (limit images ≤6), Thumbnails on web |
| 2 | What info to show | ✅ Decided | Follow prototype exactly (name, desc, size/material, packages) |
| 3 | What is 3ds Max file | ✅ Explained | Editable 3D source file — sold as premium deliverable |
| 4 | What happens after package selection | ❓ Ask client | Download immediately? Link to checkout? |
| 5 | Size & Material Selection UI | ❓ Ask client | Not clearly visible in prototype — needs clarification |
| 6 | CTA per package | ❓ Ask client | Depends on business model |
| 7 | Quantity Selector | ✅ Decided | NO — digital files don't need quantity |
| 8 | Favorite Button | ✅ Decided | YES — in prototype |
| 9 | Related Designs | ✅ Decided | Recommend to client, not in prototype — Phase 2 |
| 10 | Share Button | ✅ Decided | Recommend to client, not in prototype — ask to add |
| 11 | Reviews / Ratings | ✅ Decided | NO in Phase 1. Add Phase 2. Display nothing for now |
| 12 | Screen Header | ✅ Decided | Back arrow + Share icon (recommend). NO duplicate title |
| 13 | Login requirement | ✅ Decided | Browse without login. Login at checkout only |
| 14 | Price Computation | ❓ Ask client | Depends on business model — ask client |
| 15 | Sticky footer CTA | ✅ Decided | YES — sticky footer with primary CTA |

---

## Decision 1 — Image Slider

### Mobile (Prototype shows dots)
The prototype uses **dot indicators** below the image slider.

**Problem identified:** If too many images (8+), dots become clustered and look ugly on screen.

**Our recommendation:**
- Limit images per design to **maximum 6**
- With ≤6 images: dots work fine, clean look
- With >6 images: switch to **numbered indicator** (e.g. "2 / 6") — cleaner, no cluster

**Recommended limit: 4–6 images per design**

Reasoning:
- 4 images → clean, fast to load, dots look great
- 6 images → still acceptable with dots
- 8+ images → dots look bad, indicator approach needed

**Decision: Ask client — "How many images per design will you upload?" — then we decide dots vs counter.**

### Web (No prototype — we recommend best practice)
Dots are bad on web — too small for mouse interaction.

**Web recommendation:**
- Thumbnail strip below main image (like most e-commerce sites — Amazon, IKEA, etc.)
- User clicks thumbnail → main image updates
- Clean, professional, no clicking tiny dots
- Works perfectly for 4–8 images

### Open Questions
- [ ] How many images does client plan to upload per design? (4, 6, or 8?)
- [ ] Will there be a mix of photos + renders? Or renders only?

---

## Decision 2 — What Design Information to Show

**Rule: We follow the prototype exactly. No additions, no removals, unless we recommend and client approves.**

### What the prototype shows:
1. **Design Name** — title of the design (e.g. "Modern Booth 3x3")
2. **Description** — short text describing the design
3. **Size / Material concept** — shown as tags or text (e.g. "3x3 | Wood + Aluminum")
4. **Choose Your Package** — 3 package options side by side
   - Selected package gets a golden/highlighted border
   - Options: Basic | Premium | Customization
5. **Package deliverables shown** (what each package includes):
   - Images + PDF
   - 3ds Max File *(see below for explanation)*
   - Customize your design in 3D

### What is a 3ds Max File?
> **3ds Max** (full name: Autodesk 3ds Max) is professional 3D design software used by architects, interior designers, and booth designers.
> A **.max file** is the raw editable source file of the entire 3D design.
>
> **In plain terms:** It's like giving someone the original Photoshop `.psd` file instead of just a `.jpg` export. The buyer (another designer or company) can open it in 3ds Max, modify every element — change materials, resize, add/remove parts — and use it for their own projects.
>
> **Who buys this:** Professional design studios, exhibition companies, large corporate clients who have their own design team and want to modify the source.
>
> **This means:** The client (your app's owner) is not just selling pretty images — they're selling **professional design assets**. This is a B2B play as well, not just B2C.

### What "Customize your design in 3D" means (Screen 6)
This is the interactive 3D environment where the end user can:
- Rotate and zoom the 3D design in real-time
- Change colors (wall color, furniture, panels)
- Change materials (wood → metal → glass)
- Adjust sizes (booth 3x3 → 4x4 → 5x8)
- Add/remove elements (logo, lighting, panels)
- Walk inside the design (first-person view)
- Save their customized version → proceed to checkout

**We need to ask client:** After customization → what happens?
- Does the user get a rendered PDF of their custom version?
- Does the client receive the custom spec and produce it?
- Is there an extra charge for customization vs premium?

---

## Decision 3 — What Happens After Package Selection

User taps Basic / Premium / Customization → then what?

**Scenario A: Digital Download**
→ User buys → gets download link immediately
→ Makes sense for "Images + PDF" and "3ds Max file" packages

**Scenario B: Consultation / Order**
→ User submits request → client team contacts them
→ Makes sense if it's custom production (building a booth for them)

**Scenario C: Both**
→ Digital download for Basic/Premium
→ Consultation flow for Customization package

**We think Scenario C is most likely based on what the app offers, but this MUST be confirmed by client.**

### Open Question for Client:
- [ ] After user selects Basic package → do they download immediately after payment, or does your team get in touch?
- [ ] After user selects Premium package → same question?
- [ ] After user customizes in 3D → what is the deliverable? Rendered PDF? Physical production? Quote request?

---

## Decision 4 — Size & Material Selection UI

**Observation:** In the prototype, size and material appear as **information** (displayed text), not as interactive selection dropdowns.

**Our position:** Don't add selection UI unless client confirms it.

If client sells fixed designs (one size, one material per design) → display only, no selection needed.
If client sells variable designs (user picks 3x3 or 4x4, wood or aluminum) → selection UI needed.

### Open Question for Client:
- [ ] Does the user choose a size/material when buying? Or are they fixed per design?
- [ ] Example: "Booth A comes in 3x3 only" vs "Booth A available in 3x3, 4x4, 5x8 — user picks"

---

## Decision 5 — CTA (Call to Action) Per Package

**Status: Cannot finalize until we know the business model (Decision 3).**

**Likely CTAs based on scenario:**
| Package | Likely CTA |
|---|---|
| Basic | "Buy Now — AED XXX" → Checkout → Download |
| Premium | "Buy Now — AED XXX" → Checkout → Download |
| Customization | "Customize in 3D" → Screen 6 (3D viewer) → Save → Checkout |

**Sticky Footer Recommendation:**
- Primary CTA button stays **fixed at the bottom** of the screen (sticky footer)
- User scrolls through details above — CTA is always visible
- This is standard e-commerce pattern — proven to increase conversions
- Much better than requiring user to scroll back up to buy

---

## Decision 6 — Quantity Selector

**Decision: NO quantity selector.**

Reasoning:
- The app sells digital files (PDF, images, 3ds Max source)
- You don't buy "2 copies" of a PDF — it makes no sense
- Even for physical production (if applicable), quantity would be handled in consultation, not a +/- button on the details screen

If client confirms physical orders → quantity can be added at checkout stage, not here.

---

## Decision 7 — Favorite Button

**Decision: YES — include it.**

- Confirmed in prototype
- Standard feature — user saves designs they like for later
- Needs: Favorites list in user profile (Screen 11)
- Technical note: Works as a toggle. If not logged in → redirect to login (or save locally until login — recommend local then sync)

---

## Decision 8 — Related Designs Section

**Not in prototype.**

**Our recommendation to client:** Add it.

Why it helps:
- User views a booth, sees 3 similar booths at the bottom → keeps browsing
- Increases session time, increases sales
- Standard on every e-commerce platform (Amazon, Etsy, IKEA)

**Phase 1:** Don't build it — focus on core screen
**Phase 2:** Add "Related Designs" as a horizontal scroll section at the bottom

**Ask client:** "Would you like a 'Similar Designs' section at the bottom? We recommend it — it increases sales."

---

## Decision 9 — Share Button

**Not in prototype — we recommend adding it.**

Why:
- User likes a booth design → shares link with their business partner or client
- Free marketing for the app — shared links bring new users
- On mobile: uses native share sheet (WhatsApp, Instagram, Copy Link)
- On web: share URL directly

**Our recommendation:** Add a share icon in the screen header (top-right).

**Ask client:** "Do you want a Share button so users can share designs with others? We recommend it."

---

## Decision 10 — Reviews / Ratings

**Not in prototype.**

**Decision: No in Phase 1. Add in Phase 2.**

Why not Phase 1:
- Requires review system backend (write, read reviews)
- Requires moderation (fake reviews, inappropriate content)
- Complex to build properly
- Not in original prototype — don't scope it unless client asks

**Phase 1:** Display nothing in this section
**Phase 2:** Add star ratings + review comments + admin moderation panel

---

## Decision 11 — Screen Header Design

**What prototype shows:** Standard back arrow on the left

**Our recommendation:**
- Back arrow (left) ← already in prototype
- Share icon (right) ← we recommend adding this
- **NO design name in header** — the name already appears below the image slider in the content area. Showing it again in the header is redundant and clutters the UI.
- We **respect the prototype** — don't add elements not asked for, except share icon which we recommend

---

## Decision 12 — Login Requirement

**Question:** What happens if a user is NOT logged in and tries to:
- Favorite a design
- Buy a package

**Two approaches:**

| Approach | Behavior |
|---|---|
| Hard gate | Block everything — must login to even browse |
| Soft gate | Browse freely, login only when needed |

**Our recommendation: Soft gate (standard e-commerce)**
- Let user browse all designs, scroll details, view packages — no login needed
- When they tap "Favorite" → prompt login (or save locally, sync on login)
- When they tap "Buy Now" / go to checkout → prompt login
- **Do NOT interrupt browsing flow with a login wall**

**Why this matters:**
- Hard gate = bad SEO (Google can't index your pages if content is behind login)
- Hard gate = high bounce rate (people leave if forced to sign up before seeing anything)
- Amazon, IKEA, Etsy — all let you browse without login. Login happens at checkout.
- Standard pattern. Proven. Recommended.

---

## Decision 13 — Price Computation / Pricing Model

**Status: Cannot decide without knowing client's business model.**

**What we know:**
- If the product is purely **digital files** (PDF, images, 3ds Max) → price is flat per package, size doesn't affect price
- If the product involves **physical production** (client builds the booth/restaurant) → price depends on size, material, quantity

**Open Questions for Client:**
- [ ] What does the user actually receive after purchasing Basic? (Files only? Or do you produce it for them?)
- [ ] What does the user actually receive after purchasing Premium? (Better files? Or more deliverables?)
- [ ] Is the "Customization" package purely digital (customized render/files) or physical production?
- [ ] Do you charge differently based on size? (3x3 vs 5x8 different price?)

**Our recommendation:** Once client confirms, we'll know whether to show:
- Simple flat price per package (if digital only)
- Size-based price table (if physical production)
- Quote request flow (if custom orders)

---

## What the App Actually Offers — Summary Understanding

Based on everything discussed, here is our current understanding of what this app does:

**The client is a UAE design company** that creates professional booth, restaurant, office, and event space designs.

**They sell three tiers of access to their designs:**

1. **Basic Package**
   - Probably: High-quality rendered images + PDF drawings
   - Target user: Small business, someone who wants to see the design and share with a contractor

2. **Premium Package**
   - Probably: Everything in Basic + the 3ds Max source file
   - Target user: Design studios, corporate clients who want to modify and produce themselves

3. **Customization Package**
   - Probably: Client uses the 3D configurator to adjust the design to their needs (size, material, colors)
   - After customization → the client either gets a custom PDF/render OR it becomes a production order
   - Target user: End clients who want a tailored version of the design

**This is a hybrid B2B + B2C product.**
- B2C: Small businesses, event organizers buying the design
- B2B: Design studios, large corporates buying source files

**⚠️ Must confirm with client — this entire understanding is our assumption based on the prototype.**

---

## Open Questions for Client (Screen 4 Specific)

- [ ] How many images per design will you upload? (4, 6, or 8?) — important for slider UI
- [ ] After paying for Basic → does user download files immediately? Or does your team get in touch?
- [ ] After paying for Premium → same question?
- [ ] After customizing in 3D → what does the user receive? (Custom PDF? Production order? Quote?)
- [ ] Does the user select size/material or is it fixed per design?
- [ ] Do you want a "Share this design" button?
- [ ] Do you want "Similar Designs" at the bottom?
- [ ] Is pricing flat per package, or does size affect the price?

---

## Technical Notes

### Image Slider (Mobile)
```
react-native-reanimated (gesture handler + smooth swipe)
react-native-snap-carousel OR FlatList with pagingEnabled
Dot indicator: react-native-pagination-dots (if ≤6 images)
Counter indicator: custom text "2 / 6" (if >6 images)
```

### Image Slider (Web)
```
Swiper.js OR keen-slider (lightweight, fast)
Thumbnail strip below main image
Keyboard navigation support (left/right arrows)
```

### Package Selector
```
3 cards side by side (horizontal scroll on small screens)
Selected card: golden border + highlighted background
Tapping card → updates CTA button in sticky footer
```

### Sticky Footer CTA
```
Position: fixed / absolute at bottom of screen
Contains: Price + primary action button
Mobile: "Buy Now — AED 299"
Updates dynamically when user switches package
```

### Favorite Button
```
Heart icon (top-right of image slider or near design title)
Toggle: filled (saved) / outlined (not saved)
State stored in: Supabase `user_favorites` table (when logged in)
Local state: AsyncStorage (when not logged in, sync on login)
```

---

## Database Schema (Partial — what this screen reads)

```sql
-- Design info
designs (
  id, category_id, name, description,
  size_info, material_info,
  created_at, updated_at
)

-- Images per design
design_images (
  id, design_id, url, sort_order,
  type ENUM('render', 'photo', 'technical_drawing')
)

-- Package options per design
design_packages (
  id, design_id,
  package_type ENUM('basic', 'premium', 'customization'),
  price, deliverables_description,
  includes_pdf, includes_3dsmax, includes_3d_configurator
)

-- User favorites
user_favorites (
  id, user_id, design_id, created_at
)
```

---

## Estimated Effort

| Task | Hours |
|---|---|
| Image slider (mobile) | 4–5 hrs |
| Image slider (web thumbnails) | 3–4 hrs |
| Design info display | 2–3 hrs |
| Package selector (3 cards, golden highlight) | 4–5 hrs |
| Sticky footer CTA | 2–3 hrs |
| Favorite button (toggle + Supabase sync) | 3–4 hrs |
| Login gate at checkout (soft gate logic) | 2–3 hrs |
| Related designs section (Phase 2) | 4–5 hrs |
| Share button | 2 hrs |
| **Phase 1 Total** | **~22–27 hrs** |

**Estimated Price: 3,500 – 4,500 AED (Phase 1, Screen 4 only)**

---

## Blockers Before Development Can Start

1. Client must confirm how many images per design → determines slider dot vs counter
2. Client must confirm what happens after purchase → determines CTA flow
3. Client must confirm if size/material is selectable → determines UI complexity
4. Client must confirm business model (digital only vs physical production) → determines pricing display
