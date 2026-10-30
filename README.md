![MalaysianGeek](public/logo.webp)

# MalaysianGeek

**The Geek Fix For Malaysians.**

A personal Malaysian technology blog: hardware reviews, tutorials, DIY builds, the local tech scene, gaming and movie tech, and the occasional strong opinion about mechanical keyboards. The site pairs a long-form magazine layout with an industrial-dark look — a fixed brick-wall backdrop, frosted cards, Playfair Display headlines, and a proper 680 px reading column.

It is a full CMS-backed site, not a static template: content, media, taxonomies, menus, widgets and comments are all managed from the EmDash admin at `/_emdash/admin`.

- **Repository:** [NexzoDev/malaysiangeek-blog](https://github.com/NexzoDev/malaysiangeek-blog)
- **Stack:** Astro 7 · EmDash 1.3 · Cloudflare Workers (D1 + R2)
- **Design brief:** [PRD.md](PRD.md) — the original "industrial tech magazine" specification

---

## Table of contents

- [Features](#features)
- [Tech stack](#tech-stack)
- [Requirements](#requirements)
- [Getting started](#getting-started)
- [Scripts](#scripts)
- [Project structure](#project-structure)
- [Routes](#routes)
- [Content model](#content-model)
- [Design system](#design-system)
- [Writing and publishing](#writing-and-publishing)
- [Deployment](#deployment)
- [Conventions for contributors](#conventions-for-contributors)
- [Known gaps and next steps](#known-gaps-and-next-steps)
- [Documentation](#documentation)

---

## Features

**Homepage**

- Magazine masthead with the site tagline and a direct RSS link.
- `FeaturedGrid` — five flagged posts arranged as a centre hero with two stacked either side on desktop; below 768 px it becomes one tall card with pagination dots, auto-advancing every 5 s, with swipe navigation and `prefers-reduced-motion` respected.
- `HomeFeed` — a "Stories" list with a Trending/Latest switcher, horizontal feed cards (category, title, excerpt, byline, reading time, tag chips), and a "Trending topics" rail built from the most-used tags plus the top categories.

**Posts and archives**

- Article layout: inline byline/date/reading-time row, optional hero image, 680 px body, and a right rail holding an auto-generated table of contents (scroll-spy) plus the `sidebar` widget area.
- Tag chips between the body and the comments, threaded comments with a comment form, and a "Continue reading" grid of recent posts.
- `/posts` archive with row cards, tag chips and cursor pagination; category and tag archives share the same pagination model.
- Pages (`/pages/:slug`) render with the title beside a frosted card on desktop, and inside the card on smaller screens.

**Discovery and metadata**

- Full-text search (SQLite FTS via `search()`): a live header search over posts and pages (the header shows a ⌘K hint), plus a `/search` results page with highlighted matching title terms (currently scoped to posts).
- RSS 2.0 feed at `/rss.xml` (latest 20 posts).
- SEO titles, descriptions, canonical URLs, Open Graph/Twitter images and JSON-LD through `EmDashHead` + `getSeoMeta`.
- Reading time calculated from Portable Text (200 wpm, with CJK character handling).
- Theme toggle in the header (dark by default, light "graphite paper" variant), persisted in a cookie; no sticky header, so pages scroll as one sheet.

---

## Tech stack

| Layer           | What is used                                                                                      |
| --------------- | ------------------------------------------------------------------------------------------------- |
| Framework       | [Astro 7](https://astro.build) with `output: "server"` and `@astrojs/cloudflare`                   |
| CMS             | [EmDash](https://emdashcms.com) 1.3 — content, media, taxonomies, menus, widgets, comments, search |
| Admin UI        | React 19 islands shipped by EmDash at `/_emdash/admin`                                            |
| Database        | Cloudflare D1 (`DB` binding)                                                                      |
| Media storage   | Cloudflare R2 (`MEDIA` binding)                                                                   |
| Styling         | Plain CSS — design tokens plus scoped Astro styles (no CSS framework)                             |
| Fonts           | Astro font pipeline: Inter (body), Playfair Display (headings), JetBrains Mono (labels/code)      |
| Language        | TypeScript, with collection types generated into `emdash-env.d.ts`                                |
| Package manager | pnpm 11 (pinned in `package.json` via `packageManager`)                                           |
| Deployment      | Cloudflare Workers via Wrangler 4                                                                 |

---

## Requirements

- **Node.js `>= 22.23.4`** — required by Astro 7 (EmDash itself requires `>= 22.16`).
- **pnpm 11** — the repo pins `pnpm@11.9.0`. Enable it with `corepack enable` or install pnpm globally.
- **A Cloudflare account** with Wrangler authenticated (`pnpm wrangler login`) for deployment.
- Windows, macOS or Linux.

---

## Getting started

```bash
pnpm install
pnpm dev
```

Then:

1. The site runs at **http://localhost:4321**.
2. Open **http://localhost:4321/_emdash/admin** and complete the setup wizard to create the first admin account.

On the first request EmDash runs its pending migrations, then applies `seed/seed.json` when the database is empty and setup has not been completed. The seed supplies the schema plus demo content: **12 published posts and 1 draft**, an **About** page, the `news` / `reviews` / `misc` categories, four tags, two bylines, the primary menu, both widget areas and the reusable sections.

Astro's integration also regenerates `emdash-env.d.ts` (typed `Post` and `Page` interfaces) when the dev server starts. To refresh it against a running site manually:

```bash
npx emdash types
```

> The dev database is a local artifact — `data.db`, `data.db-*` and `.emdash/migrations.json` are gitignored. To reset local content and re-run the seed from scratch, stop the dev server, delete `data.db*` and `.emdash/migrations.json`, then start it again.

---

## Scripts

| Command            | What it does                                                 |
| ------------------ | ------------------------------------------------------------ |
| `pnpm dev`         | Start the Astro dev server at `http://localhost:4321`        |
| `pnpm build`       | Production build for the Cloudflare adapter                  |
| `pnpm preview`     | Preview the built site locally                               |
| `pnpm deploy`      | `astro build && wrangler deploy` — build and ship to Workers |
| `pnpm typecheck`   | `astro check` — TypeScript and Astro diagnostics             |
| `npx emdash types` | Regenerate `emdash-env.d.ts` from a running site             |

---

## Project structure

```text
.
├── astro.config.mjs        # Astro + EmDash integration, D1/R2 bindings, Google fonts
├── wrangler.jsonc          # Worker name, D1 database, R2 bucket, cron trigger
├── src/
│   ├── worker.ts           # Cloudflare Worker entry
│   ├── live.config.ts      # EmDash loader registration (boilerplate — leave alone)
│   ├── layouts/
│   │   └── Base.astro      # Head/SEO wiring, header (logo, menu, live search, theme
│   │                       # toggle, admin link), footer (links + newsletter), global styles
│   ├── components/
│   │   ├── FeaturedGrid.astro   # Five-card hero: desktop grid, phone carousel
│   │   ├── FeaturedCard.astro   # Main/side hero card
│   │   ├── HomeFeed.astro       # Trending/Latest feed + trending topics rail
│   │   ├── PostCard.astro       # card | feature | feed | row variants
│   │   ├── Byline.astro         # inline avatars/names or full author list
│   │   └── Pagination.astro     # cursor-based next/first links
│   ├── pages/
│   │   ├── index.astro          # Homepage
│   │   ├── posts/index.astro    # All posts (+ ?cursor=)
│   │   ├── posts/[slug].astro   # Article: TOC, widgets, tags, comments
│   │   ├── pages/[slug].astro   # CMS pages (About, etc.)
│   │   ├── category/[slug].astro
│   │   ├── tag/[slug].astro
│   │   ├── search.astro         # FTS results page
│   │   ├── rss.xml.ts           # RSS 2.0 feed
│   │   └── 404.astro
│   ├── styles/
│   │   ├── tokens.css           # Design-token defaults (@layer base) — do not edit for theming
│   │   ├── theme.css            # Site overrides: palette, brick backdrop, frosted cards
│   │   └── prose.css            # Long-form article typography
│   └── utils/
│       ├── locale.ts            # Locale-aware date formatting
│       ├── reading-time.ts      # Portable Text word/CJK counting
│       └── site-identity.ts     # Site title/tagline/logo resolution with fallbacks
├── seed/seed.json          # Schema + demo content applied on first boot
├── public/                 # Logo, favicons, web manifest, bg-repeat.webp brick texture
├── emdash-env.d.ts         # Generated collection types (do not edit)
└── PRD.md                  # Original design brief and product requirements
```

---

## Routes

| Route              | Page          | Notes                                                                     |
| ------------------ | ------------- | ------------------------------------------------------------------------- |
| `/`                | Home          | Featured grid, Stories feed, trending topics                              |
| `/posts`           | Archive       | Row cards, tag chips, cursor pagination (`?cursor=`)                      |
| `/posts/:slug`     | Article       | Hero image, TOC, widget rail, tags, threaded comments, related posts      |
| `/pages/:slug`     | CMS page      | `/pages/about` is seeded                                                  |
| `/category/:slug`  | Category      | `news`, `reviews`, `misc`                                                 |
| `/tag/:slug`       | Tag           | `webdev`, `opinion`, `tools`, `creativity`                                |
| `/search?q=`       | Search        | FTS results with highlighted title matches                                |
| `/rss.xml`         | Feed          | Latest 20 published posts                                                 |
| `/_emdash/admin`   | Admin         | Content, media, schema, menus, widgets, comments, settings                |
| anything else      | 404           | Includes a search box                                                     |

---

## Content model

Everything below is declared in `seed/seed.json`. After the first boot the schema lives in the database and is editable in the admin — the seed is the reproducible starting point, not a runtime config file.

### Collections

| Collection | URL pattern      | Fields                                                                    | Supports                                                                     |
| ---------- | ---------------- | ------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `posts`    | `/posts/{slug}`  | `title` (required, searchable), `featured_image` (image), `content` (Portable Text, searchable), `excerpt` (text), `featured` (boolean) | drafts, revisions, preview, scheduling, search, SEO, comments (`commentsEnabled: true`) |
| `pages`    | `/pages/{slug}`  | `title` (required, searchable), `content` (Portable Text, searchable)     | drafts, revisions, preview, search, SEO                                      |

### Taxonomies

- **`category`** — hierarchical, posts only: `news`, `reviews`, `misc`.
- **`tag`** — flat, posts only: `webdev`, `opinion`, `tools`, `creativity`.

### Bylines

`EmDash Editorial` (default author) and `Guest Contributor` (`isGuest: true`). Bylines are attached per post and hydrated automatically on `entry.data.bylines`.

### Navigation, widgets and sections

- **Menu `primary`:** Home, News, Reviews, Misc, About.
- **Widget area `sidebar`** (rendered in the article rail): search, categories, tags, recent posts (5, with dates), monthly archives (6).
- **Widget area `footer`:** seeded with an "About" text block. Note that `Base.astro` currently builds the footer in markup (brand, Links column, newsletter, copyright) and does not render this area — editing footer widgets in the admin has no visible effect until the layout renders it.
- **Reusable sections:** `newsletter-signup` and `about-author`.
- **Site settings:** title `MalaysianGeek`, tagline `The Geek Fix For Malaysians`.

### How the homepage picks posts

`src/pages/index.astro` scans posts newest-first and collects those with `featured: true` (walking back through the archive if the latest page is thin), then renders the newest five in `FeaturedGrid` and the newest five posts in `HomeFeed`. **Flag at least five posts as featured** to fill the hero; "Trending" is a visual ranking by tag count and then recency, not real analytics.

---

## Design system

The look is **industrial dark**: a brick-wall texture (`public/bg-repeat.webp`) tiles across a fixed viewport under a light black wash, content sits on near-black surfaces (`#050505` / `#111111`), and cards are frosted panels with a hairline ring and a soft shadow. Dark is the default; a light "graphite paper" variant ships for the header toggle.

### Colour

- **MalaysianGeek purple `#9b1cb6`** (`--color-brand`) — buttons, links, active navigation, category labels, focus rings.
- **Tech red `#fe0001`** (`--color-accent`) — badges only (live / trending / featured). Never body text or broad surfaces.
- Neutrals: `--color-bg`, `--color-surface`, `--color-fill`, `--color-card`, `--color-text`, `--color-text-secondary`, `--color-muted`, `--color-border`, `--color-ring`.
- Every colour token is written as `light-dark(<light>, <dark>)`, so overriding with a plain colour changes both modes; use `light-dark()` in a `theme.css` override to keep them distinct.

### Typography

| Role        | Font             | Token            |
| ----------- | ---------------- | ---------------- |
| Headings    | Playfair Display | `--font-heading` |
| Body        | Inter            | `--font-body`    |
| Code/labels | JetBrains Mono   | `--font-mono`    |

Fonts are loaded through Astro's font pipeline in `astro.config.mjs` (`fonts:`). To swap a face, change the `name` bound to that `cssVariable` and keep the `<Font>` tags in `Base.astro` in sync.

### Layout

- `.frame` = shared content width (`--wide-width`, 1200 px) with `--frame-margin`/`--frame-padding`.
- `.band` + `.section` = full-width section wrappers. New sections should follow this pattern so widths line up.
- Article body column: `--content-width` (680 px); right rail: `--gutter-width` (280 px). Below 1100 px the rail collapses under the article and the table of contents is hidden.

### Theming

Design-token defaults live in `src/styles/tokens.css` inside `@layer base`. **Override them in `src/styles/theme.css`** — declarations there are unlayered, so they always win. Do not edit `tokens.css` or `Base.astro` for visual changes.

### Shared building blocks

`Base.astro` defines `.frame`, `.band`, `.section`, `.intro`, `.eyebrow`, `.chip`, `.btn`, and `.post-grid`. Cards render through `PostCard.astro` (`card`, `feature`, `feed`, `row`), with `FeaturedGrid.astro`/`FeaturedCard.astro` for the hero and `HomeFeed.astro` for the homepage feed. Hover states lift the card and zoom the image; the featured carousel slides/fades on phones.

---

## Writing and publishing

Everything happens in the admin UI at `/_emdash/admin` (or via the EmDash CLI, e.g. `npx emdash content ...` while the dev server runs).

1. **Create a post** in `Posts`, write the title, excerpt and Portable Text body (headings become the table of contents automatically).
2. **Set a featured image** — image fields are media objects, so pick from the media library rather than pasting a URL. Always supply alt text.
3. **Choose one category** (News / Reviews / Misc) and any tags. The category drives the article's label and archive link; tags render as chips and feed the trending rail.
4. **Attach a byline** and, if the story should appear in the hero, toggle **Featured**.
5. **Publish, schedule or keep as a draft** — the collection supports drafts, revisions, preview and scheduled publishing.
6. **Comments** are enabled on posts; replies render threaded. Configure moderation in the admin before launch.

Images in code are objects (`{ id, src, alt, ... }`), not strings — render them with `<Image image={...} />` from `"emdash/ui"` so Astro handles sizing and AVIF/WebP.

---

## Deployment

The site deploys to **Cloudflare Workers**. `wrangler.jsonc` declares the bindings:

| Binding | Resource                                   |
| ------- | ------------------------------------------ |
| `DB`    | D1 database `malaysiangeek-db`             |
| `MEDIA` | R2 bucket `malaysiangeek-media`            |
| cron    | `* * * * *` — EmDash maintenance trigger    |

```bash
pnpm wrangler login
pnpm deploy
```

Notes:

- The Worker entry is `src/worker.ts`; `nodejs_compat` is enabled.
- The D1 database and R2 bucket must exist in the Cloudflare account `wrangler.jsonc` points at. Deploying to your own account? Create them first and update the IDs/names:
  ```bash
  pnpm wrangler d1 create malaysiangeek-db
  pnpm wrangler r2 bucket create malaysiangeek-media
  ```
- Migrations run on the first request; the seed is applied only to an empty database. For production you will normally leave the demo seed out and author real content — export a clean seed with `npx emdash export-seed` when you want a reproducible schema.
- Sandboxed plugins need Worker Loader (`worker_loaders`) on a paid plan; the binding is commented out in `wrangler.jsonc`.
- Full production checklist: [Deploy to Cloudflare](https://docs.emdashcms.com/deployment/cloudflare/).

---

## Conventions for contributors

These keep the site consistent with EmDash's data model and Astro's rendering rules:

- **Server-rendered only.** `output: "server"` is required; no `getStaticPaths()` for CMS content.
- **Two kinds of ID.** `entry.id` is the slug used in URLs (`/posts/${post.id}`); `entry.data.id` is the database ULID used for API calls such as `getTermsForEntries()`.
- **Images are objects.** `featured_image` is `{ src, alt, ... }` (or a media reference with `meta.storageKey`), never a string.
- **Taxonomy names are exact.** Query `"category"` and `"tag"` — the seed's `name` values, not the labels.
- **Batch term lookups.** Use `getTermsForEntries(collection, ids, taxonomy)` once per page instead of `getEntryTerms()` per row.
- **Pass cache hints.** With Astro's cache enabled, set `Astro.cache.set(cacheHint)` from every content query, and use the `WithCacheHint` variants for settings, menus, taxonomies and widget areas.
- **Respect the frame.** New sections are `.band` elements with content inside a `.frame`.
- **Don't flatten the article.** The widget rail and table of contents are part of the reading experience on desktop.
- **Restyle via `theme.css`.** Tokens in `tokens.css` and the structure of `Base.astro` stay as they are.
- **Tech red is a badge colour.** No red body copy, red section backgrounds or red buttons.
- **Write real copy.** No stock blog placeholders ("Welcome to my blog", "Stay tuned for more") and no three identical demo posts on the homepage.

Agent skills for EmDash work live in `.agents/skills/` (`building-emdash-site`, `creating-plugins`, `emdash-cli`); `AGENTS.md` summarises the commands and key files.

---

## Known gaps and next steps

These are intentional stubs or unfinished content, not bugs:

- **Footer legal pages.** The footer links to `/pages/contact`, `/pages/privacy` and `/pages/terms`, but only `about` is seeded. Create those pages (or change the links) before launch.
- **Newsletter form.** The footer form is a front-end stub — submitting it shows "Thanks! Newsletter signup is coming soon." Wire it to a provider or plugin before promoting it.
- **Footer widget area is unused.** `seed.json` seeds a `footer` widget area ("About" block), but `Base.astro` hand-builds the footer, so admin edits there have no visible effect. Either render the area or drop it from the seed.
- **Comments.** Enabled on the `posts` collection and rendered on every article. Set moderation in the admin, or remove `commentsEnabled` and the comments UI together.
- **Demo content and imagery.** The seeded posts mix real enough topics (RTX 5090 in KL, Raspberry Pi home lab, 3D printer weekend) with generic template filler ("The Case for Static", "Notes on Simplicity"), all under the `EmDash Editorial` byline with Unsplash images. Replace or delete them, and swap in a real author byline, before publishing.
- **Marketing copy.** The homepage eyebrow reads "Malaysian tech, documented" while the masthead tagline comes from settings — keep both in sync when the site's positioning changes.
- **`PRD.md`** describes the full original vision (community section, user profiles, comment likes, three-column footer). Some of it is deliberately not implemented yet; treat it as the roadmap, not a spec of the current build.

---

## Documentation

- **EmDash docs:** https://docs.emdashcms.com — API, hooks, field types, deployment.
- **Docs MCP server:** this repo ships `.mcp.json`, `.cursor/mcp.json` and `.vscode/mcp.json` pointing at `https://docs.emdashcms.com/mcp`, so agents can query the live documentation.
- **Astro docs:** https://docs.astro.build — routing, images, fonts, caching.
- **Cloudflare Workers / D1 / R2:** https://developers.cloudflare.com

