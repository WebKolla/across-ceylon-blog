# Across Ceylon Theme System

This guide distils the visual language, layout rules, and component behaviours used across the Across Ceylon site so you can reproduce the brand inside other Next.js/Tailwind projects. Treat it as a living design contract between product, design, and engineering.

## 1. Brand & Experience Principles

- **Immersive adventure** – every screen should feel cinematic, leaning on wide hero imagery, gentle gradients, and serif headlines to sell the journey.
- **Warm professionalism** – pair vibrant accents with disciplined typography and generous whitespace to signal premium service.
- **Sri Lankan craft** – textures, watermarks, and colour pairings reference tea estates, coasts, and tropical foliage without literal ornamentation.

## 2. Global Tokens

All colour and typography tokens are defined as CSS custom properties in `app/globals.css:6` and exposed to Tailwind through `tailwind.config.ts:10`.

### 2.1 Colours

| Token (CSS var)        | Tailwind alias            | Hex       | HSL           | Primary usage                      |
| ---------------------- | ------------------------- | --------- | ------------- | ---------------------------------- |
| `--primary`            | `bg-primary`              | `#ff6a14` | `22 100% 54%` | CTAs, key highlights, focus ring   |
| `--primary-foreground` | `text-primary-foreground` | `#262121` | `0 0% 15%`    | Text on primary surfaces           |
| `--secondary`          | `bg-secondary`            | `#0d263f` | `210 65% 15%` | Header/footer, trust-backed panels |
| `--background`         | `bg-background`           | `#fdf8e7` | `45 85% 95%`  | Default page background            |
| `--foreground`         | `text-foreground`         | `#2b303b` | `220 15% 20%` | Primary body copy                  |
| `--muted`              | `bg-muted`                | `#fdf8e7` | `45 85% 95%`  | Alternating sections, cards        |
| `--muted-foreground`   | `text-muted-foreground`   | `#3c4352` | `220 20% 35%` | Low-emphasis copy                  |
| `--accent`             | `bg-accent`               | `#3b5c23` | `95 45% 25%`  | Sustainability call-outs           |
| `--border`             | `border`                  | `#eaded7` | `22 30% 88%`  | Card & input borders               |
| `--ring`               | `ring`                    | `#ff6a14` | `22 100% 54%` | Focus outlines                     |

Dark mode overrides live under `.dark` in `app/globals.css:34` and keep the orange primary while deepening neutrals.

### 2.2 Typography

- **Inter** (loaded via `next/font/google` in `app/layout.tsx:10`) – default UI/body font (`font-sans`).
- **Source Sans Pro** – secondary body copy (`font-[Source Sans Pro]` via Google import in `globals.css:1`).
- **Playfair Display** – hero headlines and editorial accents (`.font-playfair` helper in `globals.css:72`).

#### Type scale recommendations

| Context          | Desktop                       | Mobile      | Notes                                  |
| ---------------- | ----------------------------- | ----------- | -------------------------------------- |
| Hero headline    | `text-6xl` Playfair           | `text-4xl`  | See `app/about-us/page.tsx:48`         |
| Section headline | `text-4xl` Playfair           | `text-3xl`  | `SectionHeader` defaults               |
| Body copy        | `text-base` Inter             | `text-base` | Increase to `text-lg` for rich content |
| Eyebrow / badges | `uppercase tracking-[0.25em]` | `text-xs`   | Example `app/components/Footer.tsx:59` |

### 2.3 Layout & Spacing

- **Max container**: `max-w-6xl` (~1152px) inside most sections (`app/components/ui/section.tsx:65`).
- **Grid**: 12-column responsive grid managed through Tailwind utilities (`grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4`).
- **Spacing unit**: 4px baseline (`py-16`, `gap-8`).
- **Border radius**: `lg = 9px`, `md = 6px`, `sm = 3px` (`tailwind.config.ts:11`). Cards & buttons generally use `rounded-md` (`Button`), hero cards use `rounded-3xl`.

### 2.4 Motion & Interaction

- Custom hover classes (`.hover-elevate`, `.active-elevate-2` in `globals.css:52`) add subtle background shifts.
- Buttons share `focus-visible:ring-ring` behaviours (`app/components/ui/button.tsx:12`).
- Section separators (`WaveSeparator`, `DiagonalSeparator` in `app/components/ui/section.tsx:86`) introduce gentle parallax lines; disable for reduced motion via media query (`globals.css:108`).

## 3. Core Structural Components

1. **Section wrapper** (`app/components/ui/section.tsx`)
   - Handles backgrounds (image/light/card), trend overlays (`overlay='dark-horizontal'`), and decorative watermarks (`watermark='island'|...`).
   - Built for stacking: top/bottom separators, `contentClassName` for inner containers.

2. **SectionHeader** (`app/components/ui/section-header.tsx`)
   - Provides consistent headline/body alignment with optional eyebrow copy.

3. **Buttons** (`app/components/ui/button.tsx`)
   - Variants: `default` (orange fill), `secondary` (deep blue), `outline` (frosted/backdrop style), `ghost` (text-only), `destructive` (error red).
   - Sizing is `min-h-*` to accommodate AI-generated content without breaking rhythm.

4. **Badge** (`app/components/ui/badge.tsx`)
   - Typically used for meta labels and filter chips. For hero contexts, wrap in uppercase with custom tracking.

5. **Cards** (`app/components/ui/card.tsx`)
   - 1px translucent borders, subtle shadow. For glassmorphism, wrap with `bg-white/80 backdrop-blur` as seen in hero/pillar cards (`app/about-us/page.tsx:125`).

6. **Navigation/Header**
   - Sticky, translucent bar with frosted effect. Active links highlighted using primary accent; mobile drawer inherits same tokens.

7. **Footer** (`app/components/Footer.tsx`)
   - 4-column grid with social icons, contact block featuring WhatsApp links. Background uses `bg-secondary` (deep blue) with muted text.

8. **Forms** (`app/contact-us/page.tsx:310`)
   - Inputs use translucent backgrounds (`bg-white/10`, `border-white/30` on dark hero) and emphasise generous spacing (`space-y-6`).

## 4. Signature Patterns

### 4.1 Hero Storytelling

- Full-height sections with background imagery + dark horizontal overlay for legibility (`overlay="dark-horizontal"`).
- Eyebrow badge → headline (Playfair) → supporting copy → dual CTAs (solid + outline).
- Use `max-w-2xl` text width to keep narratives readable.

### 4.2 Alternating Bands

- Alternate `bg-background` and `bg-muted` to create rhythm.
- Insert watermarks for subtle texture; keep opacity under `0.06`.
- Use `py-16`/`py-20` vertical rhythm.

### 4.3 Iconography

- `lucide-react` icons sized `w-5 h-5` (`Footer`, `SectionHeader`) with `text-primary` to connect theming.
- For value props, wrap icons inside circular `bg-primary/10` containers (`app/about-us/page.tsx:111`).

### 4.4 Call-to-Action Banners

- Soft gradient backgrounds (`bg-gradient-to-br from-primary/5 via-white to-white`).
- Eyebrow text, bold heading, paragraph, then stacked CTAs.

### 4.5 Media & Galleries

- Use `rounded-3xl`, `shadow-lg`, and `object-cover` for immersive imagery (`app/about-us/page.tsx:138`).
- Pair with `grid grid-cols-2 gap-4` for stat blocks.

### 4.6 Map/Location Blocks

- Keep address copy in `whitespace-pre-line` to preserve formatting.
- Pair icons with stacked text using `flex items-start space-x-3` patterns.

## 5. Imagery & Texture

- Prefer warm, sunlit photography of cyclists, tea plantations, and coastal roads. Avoid stocky over-saturation.
- Apply light gradient overlays to maintain contrast with overlaid text.
- Use wave/diagonal separators to echo motion.

## 6. Applying the Theme Elsewhere

1. **Copy token setup**
   - Move the CSS custom properties from `app/globals.css:6-69` and the font import to your target project.
   - Replicate `tailwind.config.ts:11-74` colour/font extensions (or copy the entire `extend` block).

2. **Install dependencies**
   - Tailwind plugins: `@tailwindcss/typography`, `tailwindcss-animate`.
   - Icon library: `lucide-react`.
   - Fonts: Inter, Source Sans Pro, Playfair Display.

3. **Port structural components**
   - Bring over `Section`, `SectionHeader`, `Button`, `Badge`, `Card`, and any bespoke layout utilities that enforce spacing/texture.
   - Preserve helper classes (`hover-elevate`, watermark utilities) from `globals.css`.

4. **Adopt layout conventions**
   - Use `max-w-6xl mx-auto px-4 sm:px-6 lg:px-8` for main content wrappers.
   - Stack sections with `py-16` or `py-20` and alternate `bg-background` / `bg-muted`.

5. **Hero & CTA assembly**
   - Compose hero sections with `Section` `variant="image"`, `overlay='dark-horizontal'`, `watermark='coast'`, and dual buttons to mirror the Across Ceylon style.

6. **Accessibility**
   - Keep focus states intact by retaining `focus-visible:ring-ring` utilities.
   - Maintain colour contrast (primary orange on warm cream meets WCAG AA for large type; use deep blue for regular text when on light backgrounds).

## 7. Content & Voice Notes

- Leads with exploratory language (“Discover”, “Ride”, “Coast to coast”).
- Keeps copy concise with action-led CTAs (“Plan with an expert”).
- Reinforces credibility via social proof, CSR highlights, and WhatsApp contacts.

## 8. Quick Reference Checklist

- [ ] CSS variables + Tailwind theme imported.
- [ ] Fonts loaded (Inter, Source Sans Pro, Playfair Display).
- [ ] `Section` component or equivalent wrapper available.
- [ ] Button/Badge/Card variants theme-aware.
- [ ] Watermark utility classes copied.
- [ ] Default layout spacing + max width applied.
- [ ] Imagery curated with warm, natural grading.
- [ ] Motion helpers (`hover-elevate`, wave separators) included or intentionally replaced.

By following the tokens and compositional rules above, you can apply the Across Ceylon look-and-feel to any project while staying faithful to the original brand expression.
