# Screen 2 — Home (Categories)
> Status: ✅ Analyzed
> Estimated Effort: 20 hours
> **Quoted Price: 3,000 AED** (Web + iOS + Android)
> Back to [INDEX.md](INDEX.md)

---

## ⚡ DECISIONS SUMMARY (Quick Reference)

| Decision | Final Resolution |
|---|---|
| **Mobile Header** | • Follow client's prototype UI exactly — no modifications<br>• Respects their existing design vision<br>• Typical: Logo + Search icon + Avatar icon |
| **Web Header** | • Responsive navbar covering desktop, tablet, mobile breakpoints<br>• Desktop: full horizontal navigation<br>• Mobile: collapses to hamburger menu or hidden in favor of bottom tabs<br>• **PENDING CLIENT DISCUSSION**: Should match app brand or separate web design? |
| **Bottom Nav Tabs** | • Home (categories)<br>• Favorites (saved designs)<br>• Cart (add to cart before checkout)<br>• Profile (orders, downloads, account)<br>• Architecture foundation for entire app |
| **Category Grid Layout** | • 2-column grid with large, premium images<br>• Category name overlaid at bottom with dark gradient<br>• Matches luxury design aesthetic |
| **Hero Banner** | • 1 static promotional banner above category grid<br>• Client provides image<br>• Gives marketing/promotional space<br>• **PENDING APPROVAL**: Yes or no |
| **Categories Management** | • Backend + Database with admin panel capability<br>• Client confirmed they want to manage categories themselves<br>• Categories updated without developer intervention<br>• Uses Supabase built-in admin UI initially |
| **Database Choice** | • **Supabase** (PostgreSQL + Auth + Storage)<br>• Why: PostgreSQL structure, built-in Auth (no coding from scratch), built-in Storage (images, 3D models, PDFs), built-in admin dashboard<br>• Free tier covers launch, scales to $25/mo Pro<br>• Faster to market than raw PostgreSQL |
| **Backend Hosting** | • **Railway** ($5–20/month)<br>• Why: Simple NestJS deployment, good DX, zero DevOps complexity<br>• Alternative: Render, Fly.io (similar)<br>• Scale to AWS later if needed |
| **Web Hosting** | • **Vercel** (free tier + paid if needed)<br>• Why: Built for Next.js, zero config, already using (zylosmart, notes app)<br>• Automatic deployments, excellent performance |
| **File Storage** | • Supabase Storage (simpler) OR Cloudflare R2 (zero egress fees)<br>• R2 better for large 3D files (5–50MB)<br>• Decision: Use Supabase at launch, migrate to R2 if expensive later |
| **Arabic/RTL Support** | • **CRITICAL DECISION — PENDING CLIENT ANSWER**<br>• If YES: All UI flips right-to-left, doubles work on every screen<br>• If NO: English only, standard left-to-right<br>• UAE market usually expects Arabic — must clarify |
| **Skeleton Loading** | • Yes — animated placeholder cards instead of spinner<br>• Better perceived performance<br>• More professional for a visual/design app |
| **Pull to Refresh** | • Yes — standard mobile behavior<br>• User pulls down to refresh categories list<br>• Easy to implement, expected UX |
| **Search Bar** | • **PENDING CLIENT ANSWER**<br>• Option A: Search all designs across all categories<br>• Option B: No search (users browse categories)<br>• Affects API design and backend complexity |

---

## What the Client Said
> "User sees Categories → User clicks on Booths"

---

## Categories (from client)
1. Restaurants
2. Cosmetics
3. Booths
4. Exhibitions
5. Offices & Workspaces
6. Seasonal Events
> Client confirmed: open to adding more categories later

---

## All Decisions

| Decision | Resolution |
|---|---|
| Mobile header | Follow client's prototype UI exactly |
| Web header | Full responsive navbar — style to be discussed with client |
| Bottom nav tabs | Home, Favorites, Cart, Profile |
| Category layout | 2-column grid with large images + name overlay (dark gradient) |
| Hero banner | 1 static banner above grid (client provides image) — pending approval |
| Category management | Backend + Database — client confirmed they want admin panel |
| Database | **Supabase** (PostgreSQL + Auth + Storage + built-in dashboard) |
| Backend hosting | **Railway** (~$5–20/month) |
| Web hosting | **Vercel** (free tier, already used by developer) |
| File storage | **Supabase Storage** or **Cloudflare R2** |
| Hybrid JSON option | ❌ Not recommended — project has e-commerce, needs real backend anyway |
| Arabic / RTL support | **Pending client answer** — critical decision, doubles UI work |
| Search bar on home | **Pending client answer** |
| Pull to refresh | ✅ Yes — standard, easy, good UX |
| Skeleton loading | ✅ Yes — better than spinner for a visual/design app |

---

## Header Design

### Mobile (iOS + Android)
- Follow the client's prototype UI — do not deviate
- Typical structure:
  ```
  ┌─────────────────────────────────┐
  │  [Logo / App Name]   [Search Icon] [Avatar] │
  └─────────────────────────────────┘
  ```
- No full navbar — navigation handled by bottom tab bar

### Web (Next.js)
- Full responsive horizontal navbar
- Desktop layout:
  ```
  ┌──────────────────────────────────────────────────────┐
  │  [Logo]    Home   Categories   About    [Login] [Cart] │
  └──────────────────────────────────────────────────────┘
  ```
- On mobile web: header collapses to hamburger menu OR hides in favour of bottom tab bar pattern
- ONE responsive component covering all breakpoints

**Open question for client:**
> "For the website version, should the header match your app's brand style, or do you have a separate design vision for the web?"

---

## Bottom Navigation Bar

| Tab | Icon | Navigates To |
|---|---|---|
| Home | House icon | Categories Screen (Screen 2) |
| Favorites | Heart icon | Saved/favorited designs list |
| Cart | Shopping bag icon | Cart → Checkout (Screen 9) |
| Profile | Person icon | Profile screen (Screen 11) |

**Architectural note:** The bottom nav is a wrapper around the entire app.
It must be set up now — it affects every single screen. This is done once in `AppNavigator.tsx` and wraps all tab screens.

---

## Category Card Design

Each card contains:

| Element | Required | Notes |
|---|---|---|
| Category image | ✅ Yes | Client provides OR developer sources stock images |
| Category name (text overlay) | ✅ Yes | White text on dark gradient at bottom of image |
| Number of designs badge | Optional | e.g. "12 Designs" — good UX, shows content volume |
| "Coming Soon" overlay | If needed | For categories with no designs at launch |

**Open question for client:**
> "Will you provide images for each category, or should we use professional stock photography? Also — will all 6 categories have designs ready at launch?"

---

## Screen Layout (Full)

```
┌─────────────────────────────┐
│  [Logo]    🔍  [Avatar]      │  ← Header (follows prototype)
├─────────────────────────────┤
│  ┌─────────────────────────┐│
│  │   Hero Banner (optional) ││  ← Client provides image
│  └─────────────────────────┘│
├─────────────────────────────┤
│  Categories                 │  ← Section title
│                             │
│  ┌──────────┐ ┌──────────┐  │
│  │  [image] │ │  [image] │  │
│  │Restaurant│ │Cosmetics │  │  ← 2-col grid
│  └──────────┘ └──────────┘  │
│  ┌──────────┐ ┌──────────┐  │
│  │  [image] │ │  [image] │  │
│  │  Booths  │ │Exhibition│  │
│  └──────────┘ └──────────┘  │
│  ┌──────────┐ ┌──────────┐  │
│  │  [image] │ │  [image] │  │
│  │ Offices  │ │ Seasonal │  │
│  └──────────┘ └──────────┘  │
├─────────────────────────────┤
│  [🏠 Home][❤️ Fav][🛒 Cart][👤 Profile] │  ← Bottom nav
└─────────────────────────────┘
```

---

## States to Handle

| State | What to Show |
|---|---|
| Loading (fetching categories) | Skeleton cards — grey animated placeholder boxes |
| Success | Category grid |
| No internet / API error | "Check your connection" message + Retry button |
| Category has no designs | "Coming Soon" label overlay on card |
| Pull to refresh | Spinner at top, re-fetch categories |

---

## Backend & Database

### API Endpoints (for this screen)
```
GET /api/categories
  → Returns all active categories, ordered by display_order
  → Response: [ { id, name, image_url, design_count, is_active } ]
  → Used by: Home screen on load + pull-to-refresh

GET /api/categories/:id/designs
  → Returns paginated designs for a category
  → Used by: Screen 3 (Design List)
```

### Database Tables (designed here, used across many screens)

```sql
-- Categories table
CREATE TABLE categories (
  id             UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name           VARCHAR(100) NOT NULL,        -- "Booths"
  name_ar        VARCHAR(100),                 -- "أكشاك" (if Arabic needed)
  image_url      TEXT NOT NULL,               -- Supabase/S3 URL
  display_order  INTEGER DEFAULT 0,           -- controls order on screen
  design_count   INTEGER DEFAULT 0,           -- cached count for badge
  is_active      BOOLEAN DEFAULT true,        -- hide without deleting
  created_at     TIMESTAMP DEFAULT NOW(),
  updated_at     TIMESTAMP DEFAULT NOW()
);

-- Designs table (used from Screen 3 onwards)
CREATE TABLE designs (
  id             UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  category_id    UUID REFERENCES categories(id) ON DELETE CASCADE,
  name           VARCHAR(200) NOT NULL,
  name_ar        VARCHAR(200),
  description    TEXT,
  thumbnail_url  TEXT,                        -- shown in list view (Screen 3)
  images         TEXT[],                      -- array of URLs for slider (Screen 4)
  size_info      JSONB,                       -- { "options": ["3x3","4x4","5x8"] }
  materials      TEXT[],                      -- ["Wood", "Metal", "Glass"]
  model_3d_url   TEXT,                        -- GLB/GLTF file URL (Screen 6)
  is_active      BOOLEAN DEFAULT true,
  created_at     TIMESTAMP DEFAULT NOW(),
  updated_at     TIMESTAMP DEFAULT NOW()
);

-- Favorites table (needed for bottom nav Favorites tab)
CREATE TABLE favorites (
  id             UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id        UUID REFERENCES users(id) ON DELETE CASCADE,
  design_id      UUID REFERENCES designs(id) ON DELETE CASCADE,
  created_at     TIMESTAMP DEFAULT NOW(),
  UNIQUE(user_id, design_id)                  -- prevent duplicate favorites
);

-- Cart table (needed for bottom nav Cart tab)
CREATE TABLE cart_items (
  id             UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id        UUID REFERENCES users(id) ON DELETE CASCADE,
  design_id      UUID REFERENCES designs(id) ON DELETE CASCADE,
  package        VARCHAR(50) NOT NULL,        -- 'basic' | 'premium' | 'customization'
  quantity       INTEGER DEFAULT 1,
  created_at     TIMESTAMP DEFAULT NOW()
);
```

---

## Infrastructure Decisions

### Database: Supabase

**Why Supabase over raw PostgreSQL or MongoDB:**
- PostgreSQL underneath — proper relational database for categories → designs → orders
- Built-in Auth system (users, login, JWT) — no need to build it from scratch
- Built-in Storage — upload category images, 3D models, PDFs in one place
- Built-in Admin dashboard — client can manage categories directly in Supabase UI without a custom admin panel at launch
- Row Level Security — users can only see their own orders/favorites
- Free tier covers launch: 500MB DB, 1GB storage, 50,000 monthly active users
- Scales to $25/month Pro plan when needed

### Backend Hosting: Railway
- Deploy NestJS API directly
- Simple GitHub integration — push code → auto deploy
- $5–20/month depending on usage
- No DevOps complexity at launch

### Web Frontend: Vercel
- Best platform for Next.js (same company)
- Zero config deployment
- Free tier covers launch
- Already used by developer (zylosmart, notes app)

### File Storage: Cloudflare R2 (or Supabase Storage)
- Cloudflare R2: zero egress fees — important when serving large 3D model files (5–50MB each)
- Supabase Storage: simpler if already on Supabase
- Decision: use Supabase Storage at launch, migrate to R2 if 3D files become expensive

### Monthly Cost for Client (Recurring)

| Service | Plan | AED/month |
|---|---|---|
| Supabase | Free → Pro | 0–92 AED |
| Railway | Starter | 18–74 AED |
| Vercel | Hobby → Pro | 0–74 AED |
| Domain name | Annual | ~37 AED/month |
| **Total** | | **~55–277 AED/month** |

**Recommendation to client:**
> "Option A (Supabase + Railway + Vercel): ~55–277 AED/month — ideal for launch, handles thousands of users.
> Option B (Full AWS): ~300–750 AED/month — enterprise scale, not needed until 10,000+ active users."

---

## Navigation When Category is Tapped

```
User taps "Booths"
  → navigate('DesignList', {
      categoryId: 'uuid-here',
      categoryName: 'Booths'
    })
  → Screen 3 uses categoryId to call GET /api/categories/:id/designs
  → Screen 3 header title shows categoryName ("Booths")
```

---

## Packages Needed

### React Native (iOS + Android)
```
@react-navigation/bottom-tabs     → bottom navigation bar
@react-navigation/native-stack    → stack navigation inside tabs
expo-linear-gradient              → gradient overlay on category cards
react-native-fast-image           → optimized image loading + caching
@supabase/supabase-js             → database + auth client
react-native-skeleton-placeholder → skeleton loading cards
```

### Web (Next.js)
```
@supabase/supabase-js   → database client
@supabase/auth-helpers  → auth helpers for Next.js
framer-motion           → smooth animations + skeleton loading
next/image              → optimized image loading
```

---

## File Structure

```
src/
  screens/
    HomeScreen.tsx              ← category grid + hero banner
  components/
    CategoryCard.tsx            ← individual category card component
    HeroBanner.tsx              ← optional banner at top
    SkeletonCategoryCard.tsx    ← loading placeholder
    BottomNavBar.tsx            ← bottom tab navigation (shared across app)
  navigation/
    AppNavigator.tsx            ← main app navigator with bottom tabs
    TabNavigator.tsx            ← tab bar definition
  api/
    categories.ts               ← API call: GET /api/categories
  hooks/
    useCategories.ts            ← custom hook: fetch + cache + refresh categories
```

---

## Effort Breakdown — By Platform

### iOS + Android (React Native + Expo)
| Task | Hours | Reasoning |
|---|---|---|
| Bottom tab navigation setup (wraps whole app) | 3 hrs | This is architectural — bottom nav wraps every screen in the app. Must be done correctly now. Involves tab bar component, icons, active state styling, linking all 4 tabs to their screens |
| Header component (follows prototype UI) | 1.5 hrs | Logo, search icon, avatar icon — responsive, styled to match prototype |
| Hero banner component | 1 hr | Static image from client, full-width, touch to navigate |
| Category grid + CategoryCard component | 3 hrs | 2-column FlatList, card with image + gradient overlay + text, touch feedback |
| Skeleton loading cards | 1.5 hrs | Animated placeholder cards for loading state — 3 rows of 2 |
| Error state + retry | 0.5 hrs | "No connection" UI + retry button |
| "Coming Soon" overlay on empty category | 0.5 hrs | Conditional overlay on card |
| API integration (GET /api/categories) | 1 hr | useCategories hook, loading/error/success states |
| Pull to refresh | 0.5 hrs | RefreshControl on FlatList |
| Navigation on tap (pass categoryId + name) | 0.5 hrs | Navigate to Screen 3 with params |
| Testing iOS + Android | 1 hr | Grid layout, image loading, tap navigation, loading states |
| **Mobile Subtotal** | **14 hrs** | |

### Web (Next.js)
| Task | Hours | Reasoning |
|---|---|---|
| Responsive header / navbar (desktop + mobile) | 2.5 hrs | Desktop: full nav. Tablet: condensed. Mobile: hamburger or hidden. One component, 3 breakpoints |
| Hero banner | 0.5 hrs | next/image, full-width, responsive |
| Category grid (CSS Grid, responsive) | 2 hrs | 2-col on mobile, 3-col on tablet, 4-col on desktop |
| Skeleton loading (web) | 0.5 hrs | CSS animation placeholders |
| API integration (same endpoint, different client) | 0.5 hrs | Supabase JS client for Next.js |
| **Web Subtotal** | **6 hrs** | |

### Total
| | Hours | Rate | Cost |
|---|---|---|---|
| Mobile (iOS + Android) | 14 hrs | 150 AED/hr | 2,100 AED |
| Web | 6 hrs | 150 AED/hr | 900 AED |
| **Total** | **20 hrs** | | **3,000 AED** |

---

## Pricing Justification (If Client Asks)

> **"Why does a Home screen cost 3,000 AED?"**
>
> Screen 2 is not just a list of 6 icons. It includes:
> - **Bottom navigation bar** built for the entire app (Home, Favorites, Cart, Profile) — this is foundational architecture that every other screen depends on
> - **Responsive web header** with 3 breakpoints (desktop, tablet, mobile)
> - **Database design** for categories, designs, favorites, and cart — 4 tables that power the entire app's data layer
> - **API endpoint** for fetching categories with proper error handling, caching, and pagination
> - **3 UI states**: loading (skeleton cards), success (grid), error (retry prompt)
> - **Pull-to-refresh**, touch navigation, image optimization across 3 platforms
>
> It is built once. It runs on iOS, Android, and Web simultaneously.
>
> **Rate: 150 AED/hour** — standard UAE freelance rate for full-stack React Native + Next.js + Supabase development

---

## Open Questions for Client — Screen 2

- [ ] Will you provide category images, or should we use professional stock images?
- [ ] All 6 categories have designs ready at launch, or some are "Coming Soon"?
- [ ] Arabic language support needed? (RTL — this doubles UI work on every screen)
- [ ] Hero banner above categories — yes or no?
- [ ] 2-column grid approved, or do you prefer full-width stacked cards?
- [ ] Web header — match app style or separate design?
- [ ] Search bar on home — search all designs across categories?
- [ ] Budget for monthly hosting: Option A (~55–277 AED/mo) or Option B AWS (~300–750 AED/mo)?
- [ ] Favorites — can users save designs to a favorites list? (Heart icon in bottom nav)
- [ ] Cart — can users add multiple designs before checkout, or purchase one at a time?

---

## Notes & Decisions Log

- `May 11` — Screen fully analyzed. Bottom nav confirmed as: Home, Favorites, Cart, Profile.
- `May 11` — Mobile header follows client prototype. Web header needs separate discussion.
- `May 11` — Database stack decided: Supabase (PostgreSQL + Auth + Storage).
- `May 11` — Hosting decided: Railway (backend) + Vercel (web) + Supabase (DB).
- `May 11` — Hybrid JSON option ruled out — project has e-commerce so real backend is required regardless.
- `May 11` — 4 DB tables designed: categories, designs, favorites, cart_items.
- `May 11` — Pricing: 3,000 AED (20 hrs × 150 AED/hr). Covers Web + iOS + Android.
