# Screen 3 — Design List (Grid)
> Status: ✅ Analyzed
> Estimated Effort: 20.5 hours
> **Quoted Price: 3,100 AED** (Web + iOS + Android)
> Back to [INDEX.md](INDEX.md)

---

## ⚡ DECISIONS SUMMARY (Quick Reference)

| Decision | Final Resolution |
|---|---|
| **Design Card Contains** | • High-quality thumbnail image<br>• Design name (e.g. "Modern Booth A")<br>• Price display: "From X AED" (starting price)<br>• Favorite heart button with optimistic UI<br>• Optional: New/Popular badges |
| **Favorite Button** | • ✅ YES — directly on card (not just detail page)<br>• Optimistic UI: heart fills instantly<br>• API call in background<br>• Rollback on error + toast notification<br>• Login gate: redirect to login if not authenticated first |
| **Grid Layout** | • **2-column grid (locked to client's prototype)**<br>• No deviations — client already approved prototype<br>• Responsive: 2-col (mobile), 3-col (tablet), 4-col (web desktop)<br>• High-performance using @shopify/flash-list |
| **Sorting Options** | • **PENDING CLIENT DISCUSSION**<br>• Possible: Sort by newest, popular, price low-to-high<br>• Affect API query params<br>• Client preference will determine which to build |
| **Filtering Options** | • **PENDING CLIENT DISCUSSION**<br>• Possible: Filter by package (Basic/Premium/Custom), size, material, price range<br>• Start simple (Phase 1): Filter by package only<br>• Advanced filters in Phase 2 if needed |
| **Pagination Strategy** | • Mobile: Infinite scroll (seamless, user taps bottom = loads next)<br>• Web: Pagination or load-more button (better for SEO)<br>• Same API endpoint — just different frontend handling<br>• Both use page & limit query params |
| **Empty State** | • ✅ YES — include when category has zero designs<br>• Illustration + friendly message<br>• "Go Back to Categories" button<br>• Don't leave users stranded |
| **Header on Screen** | • Follow client's prototype exactly<br>• Back button (← returns to Home)<br>• Category name as title (e.g. "Booths")<br>• Filter icon (opens sort/filter options)<br>• Stack navigation header |
| **Image Optimization** | • Two versions per image stored:<br>• Thumbnail (compressed, ~150–300KB) for list view<br>• Full (high quality, ~1–3MB) for detail page<br>• Blur-up placeholder while loading<br>• Mobile: react-native-fast-image (aggressive caching)<br>• Web: next/image (automatic WebP + lazy load) |
| **Price Display** | • Show "From X AED" for starting price<br>• Gives users instant price visibility<br>• Reduces friction before tapping into details<br>• Optional: show price range (From–To) if multiple tiers |
| **Pull to Refresh** | • ✅ YES — refresh designs list<br>• Standard mobile UX<br>• RefreshControl on mobile, fetch new data |
| **New/Popular Badges** | • Optional visual tags on cards<br>• "NEW" for recently added<br>• "POPULAR" for high view count<br>• **PENDING CLIENT CONFIRMATION** — do they want these? |

---

## What the Client Said
> "Grid layout with designs. Each design includes image + name. User scrolls. User clicks on Modern Booth A."

---

## All Decisions

| Decision | Resolution |
|---|---|
| Design card contains | Image + name + price (from) + favorite heart button |
| Favorite button | ✅ YES — directly on card, optimistic UI, heart fills instantly |
| Grid layout | **Follow client's prototype exactly** — 2-column grid (client already approved prototype) |
| Responsive grid | 2-col on mobile, 3-col on tablet, 4-col on web desktop |
| Sorting | **To be discussed with client** — ask about sort preferences before building |
| Filtering | **To be discussed with client** — ask if filter by package/size/price is needed |
| Pagination vs Infinite scroll | **Infinite scroll on mobile** (smooth), **Pagination on web** (SEO-friendly) |
| Empty state | ✅ YES — include empty state with illustration + back button |
| Header on screen | **Follow client's prototype** — Back button + Category name + Filter icon |
| Image optimization | Blur-up placeholder while loading, fast-image on mobile |
| Pull to refresh | ✅ YES |
| Price display | Show "From X AED" for starting price |
| New/Popular badges | Optional — pending client confirmation if designs have these tags |

---

## Screen Layout (Per Client Prototype)

```
┌─────────────────────────────────┐
│  [← Back]     Booths   [Filter] │  ← Follow prototype header
├─────────────────────────────────┤
│  Sort: [Newest ▼]  12 Designs   │  ← Sort control (pending client preference)
├─────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐     │
│  │ [image]  │  │ [image]  │     │
│  │        ❤️ │  │        ❤️ │     │  ← Favorite button (optimistic UI)
│  │Modern    │  │Classic   │     │
│  │Booth A   │  │Booth B   │     │
│  │From 2,500│  │From 3,200│     │  ← Price
│  └──────────┘  └──────────┘     │
│  ┌──────────┐  ┌──────────┐     │
│  │ [image]  │  │ [image]  │     │
│  │        ❤️ │  │        ❤️ │     │
│  │Industrial│  │  Luxury  │     │
│  │Booth C   │  │Booth D   │     │
│  │From 4,100│  │From 5,800│     │
│  └──────────┘  └──────────┘     │
│       ↕ infinite scroll          │
├─────────────────────────────────┤
│  [🏠 Home][❤️ Fav][🛒 Cart][👤]  │  ← Bottom nav
└─────────────────────────────────┘
```

---

## Database Schema Additions

```sql
-- Added columns to designs table for this screen and future features
ALTER TABLE designs ADD COLUMN (
  price_from      DECIMAL(10,2),           -- "From X AED"
  price_to        DECIMAL(10,2),           -- optional max price
  is_popular      BOOLEAN DEFAULT false,   -- "Popular" badge
  is_new          BOOLEAN DEFAULT false,   -- "New" badge
  view_count      INTEGER DEFAULT 0        -- for "Popular" sorting
);

-- Favorites table (created in Screen 2, used heavily here)
-- id, user_id, design_id, created_at
```

---

## API Endpoints for This Screen

```
GET /api/categories/:categoryId/designs
  Query params:
    page     → default: 1 (for pagination on web)
    limit    → default: 20 (designs per page)
    sort     → 'newest' | 'popular' | 'price_asc' | 'price_desc'
              (sorting options to be finalized with client)
    package  → 'basic' | 'premium' | 'customization' (optional filter)
  Response:
    {
      designs: [
        {
          id,
          name,
          thumbnail_url,
          price_from,
          is_new,
          is_popular
        }
      ],
      total: 47,
      page: 1,
      hasMore: true
    }

POST /api/favorites
  Body: { design_id: 'uuid' }
  Response: { favorited: true }

DELETE /api/favorites/:designId
  Response: { unfavorited: true }

GET /api/favorites
  Response: { design_ids: ['uuid1', 'uuid2', ...] }
  Used by: Bottom nav Favorites tab (Screen 11)
```

---

## Favorite Button — Real-Time Behavior

When user taps the heart icon:

```
1. User NOT logged in
   → Redirect to Login screen
   → After login, return to this screen

2. User IS logged in, taps heart
   → Immediately fill/unfill heart (optimistic UI)
   → Call API in background: POST /api/favorites or DELETE
   → If success → keep state
   → If error → revert heart to previous state + show toast "Failed, try again"
```

This is a core UX pattern — the app feels instant even though the network call takes time.

---

## Pagination on Web vs Infinite Scroll on Mobile

### Mobile (iOS + Android) — Infinite Scroll
```
User scrolls down → reaches last card
  → Automatically detect scroll end
  → Load next page (API call in background)
  → Append new designs to grid
  → Show loading indicator at bottom
  → Continue scrolling seamlessly
```
**Smooth, no interruption, natural mobile UX.**

### Web (Next.js) — Pagination
```
User scrolls down → reaches last card
  → Show "Load More" button OR auto-load next page
  → OR show page numbers at bottom
  → (Better for SEO, users know where they are in the list)
```

Both use the same API endpoint (`page` query param). The difference is just frontend handling.

---

## Empty State (When Category Has No Designs)

```
┌──────────────────────────────────┐
│                                  │
│      [Empty illustration]        │
│                                  │
│    No designs in this category   │
│    Check back soon!              │
│                                  │
│  [← Go Back to Categories]       │
│                                  │
└──────────────────────────────────┘
```

**Component includes:**
- Centered illustration (client provides or use generic icon)
- Friendly message
- Back button that navigates to Screen 2 (Home)

---

## Image Optimization — Blur-Up Technique

For a design company, image quality matters. High-res design photos are large (2–5MB each). Loading them without optimization kills performance.

**Solution:**
Store two versions of every image in Supabase Storage / S3:

| Version | Size | Use |
|---|---|---|
| `thumbnail` (compressed) | ~150–300 KB | Screen 3 grid (this screen) |
| `full` (high quality) | ~1–3 MB | Screen 4 detail page slider |

**Loading flow:**
```
1. Show blurred low-res placeholder (from image metadata)
2. Load thumbnail in background (fast, ~300ms)
3. Thumbnail displays
4. If user enters detail page, load full image (can afford to wait)
```

This gives the perception of speed — user sees content immediately, high quality comes later.

**Packages:**
- Mobile: `react-native-fast-image` — aggressively caches images to disk
- Web: `next/image` — automatic WebP conversion + lazy loading

---

## Packages Needed

### React Native (iOS + Android)
```
@shopify/flash-list              → high-performance grid (60fps scrolling, 100+ items)
react-native-fast-image          → fast image loading + disk caching
react-native-reanimated          → smooth heart fill animation
react-native-gesture-handler     → touch handling for favorite button
react-native-skeleton-placeholder → grid skeleton during loading
```

### Web (Next.js)
```
next/image                       → automatic WebP + lazy loading
react-intersection-observer      → detect when to load next page (infinite scroll)
framer-motion                    → card animations on hover
```

---

## File Structure

```
src/
  screens/
    DesignListScreen.tsx          ← main grid screen
  components/
    DesignCard.tsx                ← individual card (image, name, price, heart)
    DesignGrid.tsx                ← FlatList/grid wrapper with infinite scroll
    FilterBottomSheet.tsx         ← filter/sort UI (if filters are approved)
    EmptyState.tsx                ← empty category state
    SkeletonDesignGrid.tsx        ← loading placeholders
  hooks/
    useDesignList.ts              ← fetch designs, pagination, sort, favorites
    useFavorites.ts               ← toggle favorite + local state
  api/
    designs.ts                    ← API calls for designs, favorites
```

---

## Effort Breakdown — By Platform

### iOS + Android (React Native)
| Task | Hours | Reasoning |
|---|---|---|
| Design grid (FlashList, 2-column per prototype) | 2.5 hrs | @shopify/flash-list for high-performance scrolling. Must handle 100+ items at 60fps without lag. Styling to match prototype |
| DesignCard component | 2 hrs | Reusable card: thumbnail image + gradient overlay + name + price + favorite heart. Touch feedback, hover states |
| Favorite button (optimistic UI) | 2 hrs | Heart fills instantly on tap. Simultaneous API call. Rollback on error. Login gate (redirect if not authenticated). Toast on failure |
| Sort/filter bottom sheet (pending client preferences) | 2 hrs | Bottom sheet UI, sort options, filter checkboxes. Stored in local state + reflects in API query |
| Skeleton loading grid | 1 hr | 6 placeholder cards while loading. Animated shimmer effect |
| Empty state UI | 0.5 hrs | Illustration + message + back button to Screen 2 |
| Search within category | 1 hr | Search bar filters designs client-side or via API |
| Infinite scroll (load on reach bottom) | 1.5 hrs | Detect when last card enters viewport. Fetch next page. Append to grid. Show loading indicator |
| Stack header (back + title + filter icon) | 0.5 hrs | Navigation header. Title from previous screen. Filter icon opens bottom sheet |
| Image blur-up placeholders | 1 hr | Show blurred low-res while thumbnail loads. Better perceived performance |
| Pull to refresh | 0.5 hrs | RefreshControl on FlatList |
| Testing (iOS simulator + Android emulator) | 1.5 hrs | Grid layout on different screen sizes, image loading, favorite toggle, infinite scroll, empty state |
| **Mobile Subtotal** | **16 hrs** | |

### Web (Next.js)
| Task | Hours | Reasoning |
|---|---|---|
| Responsive design grid (2/3/4 columns) | 2 hrs | CSS Grid with media queries. 2-col on mobile, 3-col on tablet, 4-col on desktop. Matches prototype |
| DesignCard component (web version) | 1 hr | Similar structure to mobile, adapted for web hover states (shadow, scale) |
| Infinite scroll with Intersection Observer | 1 hr | Detect when last card enters viewport. Trigger next page fetch. Seamless loading |
| Sort/filter UI (web dropdowns) | 1 hr | Dropdown or filter bar above grid. Cleaner than bottom sheet for web |
| Favorite toggle (heart click) | 0.5 hrs | Same API as mobile, web click handler. Heart fills/unfills |
| **Web Subtotal** | **5.5 hrs** | |

### Total
| | Hours | Rate | Cost |
|---|---|---|---|
| Mobile (iOS + Android) | 16 hrs | 150 AED/hr | 2,400 AED |
| Web | 5.5 hrs | 150 AED/hr | 825 AED |
| **Total** | **21.5 hrs** | | **3,225 AED** |

> **Rounded to 3,100 AED** for quotation (conservative estimate).

---

## Pricing Justification (If Client Asks)

> **"Why does a design grid screen cost 3,100 AED?"**
>
> - This screen displays **every design across all categories** — it's one component powering Restaurants, Booths, Offices, all 6+ categories
> - **Performance engineering:** 20–100+ high-quality design images in a responsive grid. Built wrong = lag, crashes, bad UX. Built right = 60fps on any device. Uses @shopify/flash-list (the same library Shopify uses), image caching, blur-up loading
> - **Favorite system:** Real-time heart toggle with optimistic UI, database write, error rollback, login gate — synchronizes across all 3 platforms and the Favorites tab (Screen 11)
> - **Pagination + infinite scroll:** Load only what's needed. Infinite scroll on mobile (smooth), pagination on web (SEO-friendly). Same API, different frontend
> - **Filter/sort system:** Integrated sort control for future client needs (pending preferences)
> - One code. Three platforms. Zero bugs.

---

## Open Questions for Client — Screen 3

- [ ] Sorting preferences: Should users sort by newest/popular/price? Or just newest?
- [ ] Filtering: Should users filter by package type (Basic/Premium/Custom)? By size (3x3, 4x4)? By material?
- [ ] Do designs have "New" or "Popular" badges?
- [ ] How many designs per category at launch? (Affects pagination strategy — if <30 designs, pagination not needed)
- [ ] Search within category — needed?

---

## Notes & Decisions Log

- `May 11` — Screen analyzed with full depth discussion.
- `May 12` — Favorite button CONFIRMED for card — optimistic UI across 3 platforms.
- `May 12` — Grid layout LOCKED to client's prototype — 2-column, no deviation.
- `May 12` — Sorting/filtering — to be discussed with client before building.
- `May 12` — Infinite scroll (mobile) + Pagination (web) — APPROVED.
- `May 12` — Empty state INCLUDED.
- `May 12` — Header follows prototype exactly.
- `May 12` — Pricing: 3,100 AED (21.5 hrs × 150 AED/hr, rounded down). Covers Web + iOS + Android.
