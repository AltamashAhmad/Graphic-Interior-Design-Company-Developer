# App Development — Files & Information You Must Provide
> Prepared by: Altamash Ahmad
> Date: May 13, 2026
> Purpose: Complete list of every file and piece of information we need from your side to build the app

---

## What We Are Building

- **iOS app** (iPhone + iPad)
- **Android app**
- **Website** (web app — same features, same content)

All three platforms are built together and stay in sync. Any content change made in the admin panel — a new design, a price update, a new category — reflects instantly on iOS, Android, and the website at the same time.

---

## How the App Works — Everything Is Dynamic

**The entire app is content-managed.** You do not need a developer to update anything after launch.

| What | Who Controls It | How |
|---|---|---|
| Categories (add, rename, reorder, remove) | Your team | Admin panel |
| Designs (add, edit, price, remove) | Your team | Admin panel |
| Product photos | Your team | Admin panel |
| Packages and pricing | Your team | Admin panel |
| Premium files (PDF, 3ds Max) | Your team | Admin panel |
| 3D model files | Your team | Admin panel |
| Hero banner image | Your team | Admin panel |
| Category cover images | Your team | Admin panel |

**There is no fixed content in the app.** Everything you see on screen comes from the backend database. Your team controls it all.

---

> **Note:**
> This document represents the information and files we need based on our discussions so far. Since the project is still being discussed and finalised, some items may be added, removed, or changed as we align further. We will update this document as decisions are confirmed.
> Treat this as a working draft, not a final requirement list.

---

> **How to use this document:**
> This is a checklist of everything we need from you to build and launch the app.
> Go through it in your meeting, assign ownership to each item, and send us files as they become ready.
> The sooner we receive these, the sooner development can proceed.

---

## SECTION 1 — Brand & App Identity

Everything here is needed before we can design a single screen.

| # | What You Must Provide | File Format | Status |
|---|---|---|---|
| 1.1 | **App name** — the official name shown on the App Store and Google Play | Text | ☐ |
| 1.2 | **Logo file** — your company logo | SVG preferred, or PNG with transparent background | ☐ |
| 1.3 | **Brand colors** — your primary and secondary brand colors | Hex codes (e.g. #1A1A2E) or Pantone numbers | ☐ |
| 1.4 | **Brand font** — the font used in your brand materials, if any | Font file (.ttf or .otf) or font name | ☐ |
| 1.5 | **Tagline** — a short line shown under the logo on the splash screen | Text (optional — leave blank if none) | ☐ |
| 1.6 | **Arabic support decision** — will the app be in Arabic, English, or both? | Your answer in text | ☐ |

---

## SECTION 2 — Category Images

> **Categories are fully dynamic.** They are managed from the admin panel — you can add, remove, rename, reorder, or create new categories at any time after launch without involving the developer.
> For launch, just give us the starting set and the images. Everything else you control yourself.

The home screen shows a grid of your design categories. Each category needs a cover image.

| # | What You Must Provide | File Format | Notes |
|---|---|---|---|
| 2.1 | **Cover image for each category** — one image per category shown on the home screen | JPG or PNG, minimum 1200×800px, high quality | One file per category. Can be updated anytime from admin panel later |
| 2.2 | **Hero banner image** — one large wide promotional image shown above the category grid | JPG or PNG, minimum 1600×600px | Can be updated anytime from admin panel later |
| 2.3 | **Starting category list** — the categories you want live at launch | Text list | You can add more, rename, or reorder anytime from the admin panel after launch |
| 2.4 | **Category display order at launch** — which category appears first, second, third, etc. | Numbered list | Can be reordered from admin panel later |

**Starting category list — confirm names exactly as they should appear at launch:**

| # | Category Name | Confirmed ☐ / Edit name below |
|---|---|---|
| 1 | Restaurants | |
| 2 | Cosmetics | |
| 3 | Booths | |
| 4 | Exhibitions | |
| 5 | Offices & Workspaces | |
| 6 | Seasonal Events | |
| + | Add any additional launch categories here | |

> You are not locked to this list. After launch you manage categories yourself from the admin panel.

---

## SECTION 3 — Designs / Products

For **every design** you want live in the app at launch, you must provide all of the following.
Prepare one folder per design with all files inside, named clearly.

### 3A — Information

> **Design information is fully dynamic.** Every field below — name, description, price, packages, sizes, materials — is managed from the admin panel. You can add new designs, edit existing ones, change prices, or remove designs at any time after launch without involving the developer.
>
> **Two options for providing this information:**
> - **Option A:** Send us a spreadsheet before launch and we populate the initial designs for you
> - **Option B:** We give you access to the admin panel and your team enters the designs directly
>
> Either way, after launch your team fully manages all design content themselves.

| # | What You Must Provide | Format | Example |
|---|---|---|---|
| 3.1 | **Design name** | Text | "Modern Booth A" |
| 3.2 | **Short description** | Text, 2–4 sentences | "A sleek modern exhibition booth with clean lines..." |
| 3.3 | **Category** | Text (from your category list) | "Booths" |
| 3.4 | **Which packages are available for this design** | Mark each: Basic / Premium / Customization | Not every design needs all three |
| 3.5 | **Price per package** (in AED) | Numbers | Basic: 500 AED, Premium: 1,200 AED, Customization: 2,500 AED |
| 3.6 | **Available sizes** | Text list | 3×3 m, 4×4 m, 5×8 m |
| 3.7 | **Available materials** | Text list | Wood, Metal, Glass |

### 3B — Files (provide per design)

| # | What You Must Provide | File Format | Notes |
|---|---|---|---|
| 3.8 | **Product photos** — images shown in the design detail gallery | JPG or PNG, minimum 1500×1000px | Provide 4 to 6 photos per design — renders or real photos both accepted |
| 3.9 | **PDF file** (Premium package) | PDF | Technical drawings with dimensions — only for designs with Premium package |
| 3.10 | **3ds Max file** (Premium package) | .max file | Editable source file — only for designs with Premium package |

---

## SECTION 4 — 3D Customization Files

**You have confirmed your team will provide all 3D files.**
This section tells your 3D artist exactly what to prepare and in what format.

> Share this entire Section 4 with your 3D artist.

---

### 4A — The 3D Model Files (GLB Format)

> **What is GLB?** GLB is a standard 3D file format — the same model your artist creates in 3ds Max or Blender, exported in the format the app can read. Your artist will know this format.

For every design that has the 3D Customization package, your 3D artist must provide:

| # | What Must Be Provided | File Format | Notes |
|---|---|---|---|
| 4.1 | **One 3D model file per design per size** | GLB (.glb) | Each size is a separate file — see naming below |
| 4.2 | **Texture image files** (embedded inside GLB) | PNG, max 2048×2048 px | Must be embedded in the GLB, not separate files |

**File naming — your artist must follow this exactly:**

```
[design-name]_[size].glb

Examples:
  modern_booth_a_3x3.glb
  modern_booth_a_4x4.glb
  modern_booth_a_5x8.glb
  classic_restaurant_3x3.glb
  classic_restaurant_4x4.glb
```

So if a design has 3 sizes, your artist delivers 3 separate GLB files for that design.

---

### 4B — Mesh Naming Inside the GLB File (Critical)

Every part of the design that users can change (color, material, show/hide) **must have a unique name** inside the 3D file. This is how the app knows which part the user is changing.

> **Your artist sets these names inside 3ds Max, Blender, or whichever software they use — before exporting to GLB.**

**Required naming convention — your artist must name each part clearly:**

| Part Type | Example Names in the File |
|---|---|
| Walls | `WallLeft`, `WallRight`, `WallBack` |
| Counter / reception desk | `Counter`, `ReceptionDesk` |
| Ceiling | `Ceiling` |
| Panels / display panels | `Panel_Left`, `Panel_Right` |
| Logo sign / signage area | `LogoPanel`, `Signage` |
| Lighting elements | `CeilingLight`, `SpotLight_1` |
| Flooring | `Floor` |
| Any removable element | `ExtraShelf`, `DisplayStand` |

**Rule:** If a part has no name, we cannot control it from the app. Every controllable part must be named.

---

### 4C — Color Options (What You Must Provide)

The app shows users a preset palette of colors to choose from. You define this palette.

| # | What You Must Provide | Format | Notes |
|---|---|---|---|
| 4.3 | **Color palette** — the list of colors users can choose from | Hex codes (e.g. #FFFFFF for White) | Recommended 8–16 colors — your brand team decides |
| 4.4 | **Which parts can be colored** per design | Text list | e.g. "Walls, Counter, and Ceiling can each be a different color" |
| 4.5 | **Default color per part** — what color each part starts at when user opens the 3D view | Hex code per part | e.g. Walls default to White (#FFFFFF), Counter defaults to Gold (#C9A84C) |

**Example color palette format to send us:**

```
White      → #FFFFFF
Black      → #1A1A1A
Gold       → #C9A84C
Silver     → #B0B0B0
Beige      → #F5F0E8
Dark Grey  → #3A3A3A
Navy Blue  → #1A2744
Cream      → #FFF8F0
```

---

### 4D — Material Options (What You Must Provide)

Materials are surface textures applied to parts of the design (e.g. a wooden counter, metal walls, glass panels).

| # | What You Must Provide | Format | Notes |
|---|---|---|---|
| 4.6 | **Material texture files** — one image file per material | PNG, minimum 1024×1024px, seamless/tileable | e.g. one file for Wood, one for Metal, one for Glass, one for Marble |
| 4.7 | **Material names** — what each material is called in the app | Text | e.g. "Oak Wood", "Brushed Metal", "Frosted Glass", "White Marble" |
| 4.8 | **Which parts can have materials changed** per design | Text list | e.g. "Counter and Panels can switch material. Walls cannot." |
| 4.9 | **Default material per part** — what material each part starts at | Text | e.g. Counter defaults to "Oak Wood" |

**Texture file naming — your artist must follow this:**

```
material_[name].png

Examples:
  material_oak_wood.png
  material_brushed_metal.png
  material_frosted_glass.png
  material_white_marble.png
  material_dark_fabric.png
```

---

### 4E — Size Options (What You Must Provide)

Each size is a completely separate 3D model file. See Section 4A for the file naming.

| # | What You Must Provide | Format | Notes |
|---|---|---|---|
| 4.10 | **List of available sizes per design** | Text (dimensions in meters) | e.g. 3×3 m, 4×4 m, 5×8 m |
| 4.11 | **One GLB file per size per design** | GLB (.glb) | Already covered in 4A — confirming here for clarity |
| 4.12 | **Default size** — which size the design opens in by default | Text | e.g. "Opens in 3×3 by default" |

---

### 4F — Add / Remove Elements (What You Must Provide)

Some parts of the design can be toggled on or off by the user (e.g. add extra shelving, remove a counter).

| # | What You Must Provide | Format | Notes |
|---|---|---|---|
| 4.13 | **List of elements that can be added or removed** per design | Text list | e.g. "Extra Display Shelf", "Logo Stand", "Ceiling Spotlights", "Brochure Rack" |
| 4.14 | **Default state per element** — is it ON (visible) or OFF (hidden) when design opens? | Text | e.g. "Logo Stand is ON by default, Extra Shelf is OFF" |
| 4.15 | **These elements must be separate named meshes in the GLB file** | (Instruction for artist) | The artist names them (e.g. `ExtraShelf`, `LogoStand`) and they are hidden/shown from the app |

---

### 4G — Technical Requirements for Your 3D Artist

Your artist must meet these technical standards for the files to work in the app:

| Requirement | Specification | Why |
|---|---|---|
| Export format | **GLB only** — not OBJ, not FBX, not STL, not GLTF+bin | GLB is a single self-contained file the app reads directly |
| Polygon count | **Maximum 150,000 polygons per model** | More than this causes lag and overheating on mobile phones |
| Texture resolution | **Maximum 2048×2048 pixels per texture** | Larger textures crash on older mobile devices |
| Texture embedding | **All textures embedded inside the GLB** — no separate texture files | App cannot load external texture files |
| Mesh naming | **Every controllable part must have a unique English name** | App controls parts by their name — unnamed parts cannot be controlled |
| Scale | **Use real-world scale** — 1 unit = 1 meter | Ensures sizes display correctly |
| Origin point | **Set origin to center-bottom of model** | Ensures model appears correctly positioned in the app |
| Test before sending | **Open the file in:** https://gltf-viewer.donmccurdy.com | If it opens and looks correct here, it will work in the app |

---

## SECTION 5 — Business Information

| # | What You Must Confirm | Your Answer |
|---|---|---|
| 5.1 | **Payment method** — will users pay by card inside the app, or place an inquiry (WhatsApp / phone / email)? | ☐ Online card payment inside app / ☐ Inquiry only |
| 5.2 | **Who will manage the app content** — adding designs, changing prices, uploading files after launch? | ☐ Technical person on your team / ☐ Non-technical staff (we will make admin panel simpler) |
| 5.3 | **Existing website** — do you have one, and are we replacing it or building the app separately? | Your answer |
| 5.4 | **First-person walkthrough** — user steps inside the 3D design and looks around — needed at launch or can be Phase 2? | ☐ Required at launch / ☐ Phase 2 is fine |
| 5.5 | **User logo upload** — do you want users to upload their company logo and see it placed on the design in 3D? | ☐ Yes, include this / ☐ No |

---

## DELIVERY CHECKLIST — COMPLETE SUMMARY

Use this as your master checklist. Check off each item as files are ready to send.

### Brand Files
- ☐ App name (text)
- ☐ Logo — SVG or PNG transparent
- ☐ Brand colors — hex codes
- ☐ Brand font — font file or name
- ☐ Tagline — text (if any)
- ☐ Arabic/English language decision

### Category Files *(dynamic — managed from admin panel after launch)*
- ☐ Starting category list with exact names for launch
- ☐ Category display order at launch
- ☐ Cover image per category — JPG/PNG min 1200×800px
- ☐ Hero banner image — JPG/PNG min 1600×600px

### Per Design *(dynamic — managed from admin panel after launch)*
> Either send a spreadsheet OR enter directly into admin panel — your choice
- ☐ Design name — text
- ☐ Description — 2–4 sentences
- ☐ Category — from your list
- ☐ Packages available — Basic / Premium / Customization
- ☐ Price per package — AED
- ☐ 4 to 6 product photos — JPG/PNG min 1500×1000px
- ☐ PDF file (Premium only)
- ☐ 3ds Max file (Premium only)

### 3D Files (Per Design With Customization Package)
- ☐ One GLB file per size per design — named correctly (e.g. `modern_booth_a_3x3.glb`)
- ☐ Color palette — hex codes (8–16 colors)
- ☐ Default color per part — hex code
- ☐ Which parts can be individually colored — text list
- ☐ Material texture files — PNG min 1024×1024px, seamless
- ☐ Material names for the app — text
- ☐ Default material per part — text
- ☐ Which parts can have materials changed — text list
- ☐ List of available sizes — text (e.g. 3×3, 4×4, 5×8)
- ☐ Default size — text
- ☐ List of add/remove elements per design — text
- ☐ Default state (on/off) per element — text
- ☐ All meshes inside GLB named correctly in English

### Business Decisions
- ☐ Payment method — card or inquiry
- ☐ Who manages content after launch
- ☐ First-person walkthrough — launch or Phase 2
- ☐ User logo upload — yes or no

---

> **Send all files and answers to:** Altamash Ahmad
> **File delivery:** Share via Google Drive, WeTransfer, or any file sharing link
> **Questions?** Contact us before your meeting if any item is unclear

---

## SUMMARY — Priority Order

Please address these first — they are blocking the start of development:

| Priority | Item | Why It's Urgent |
|---|---|---|
| 🔴 1 | **Logo + Brand Colors** (1.2 + 1.3) | Needed before any screen can be designed |
| 🔴 2 | **Arabic support YES or NO** (1.5) | Affects every single screen — must decide now |
| 🔴 3 | **Who creates 3D model files** (4.1) | If you create them: 3D development can start in parallel. If we create them: cost and timeline change significantly |
| 🔴 4 | **3D artist briefing** (4D) | Brief your artist now — 3D files take time to produce and are the longest lead item |
| 🟡 5 | **Category cover images** (2.2) | Needed for home screen — can use placeholders temporarily |
| 🟡 6 | **Design list with photos + pricing** (Section 3) | Needed to populate the app with real content |
| 🟢 7 | **Payment method decision** (5.1) | Needed before checkout is built — not urgent for Phase 1 |

---

> **Send completed document + files to:** Altamash Ahmad
> **Questions?** Reply to this document with comments against each item.
