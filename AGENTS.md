This is an EmDash site -- a CMS built on Astro with a full admin UI.

## Commands

```bash
pnpm dev              # Start the Astro dev server
npx emdash types      # Regenerate TypeScript types from a running site
```

The admin UI is at `http://localhost:4321/_emdash/admin`.

## Key Files

| File                     | Purpose                                                                            |
| ------------------------ | ---------------------------------------------------------------------------------- |
| `astro.config.mjs`       | Astro config with `emdash()` integration, database, and storage                    |
| `src/live.config.ts`     | EmDash loader registration (boilerplate -- don't modify)                           |
| `seed/seed.json`         | Schema definition + demo content (collections, fields, taxonomies, menus, widgets) |
| `emdash-env.d.ts`        | Generated types for collections (auto-regenerated on dev server start)             |
| `src/layouts/Base.astro` | Base layout with EmDash wiring (menus, search, page contributions)                 |
| `src/pages/`             | Astro pages -- all server-rendered                                                 |

## Skills

Agent skills are in `.agents/skills/`. Load them when working on specific tasks:

- **building-emdash-site** -- Querying content, rendering Portable Text, schema design, seed files, site features (menus, widgets, search, SEO, comments, bylines). Start here.
- **creating-plugins** -- Building EmDash plugins with hooks, storage, admin UI, API routes, and Portable Text block types.
- **emdash-cli** -- CLI commands for content management, seeding, type generation, and visual editing flow.

## Documentation

The EmDash docs are available as an MCP server at `https://docs.emdashcms.com/mcp`. When you need to verify an API, hook, config option, field type, or pattern, call `search_docs` against the live documentation rather than relying on training-data recall. The docs reflect current behaviour; assumptions may not.

This template ships with `.mcp.json`, `.cursor/mcp.json`, and `.vscode/mcp.json` so Claude Code, Cursor, and VS Code auto-discover the docs server. Other tools (OpenCode, Windsurf, etc.) need a manual one-time setup -- see [docs.emdashcms.com/docs-mcp](https://docs.emdashcms.com/docs-mcp).

## Rules

- All content pages must be server-rendered (`output: "server"`). No `getStaticPaths()` for CMS content.
- Image fields are objects (`{ src, alt }`), not strings. Use `<Image image={...} />` from `"emdash/ui"`.
- `entry.id` is the slug (for URLs). `entry.data.id` is the database ULID (for API calls like `getEntryTerms`).
- When Astro's cache is enabled, pass content-query hints to `Astro.cache.set(cacheHint)`. Use the `WithCacheHint` variants for site settings, menus, taxonomies, and widget areas rendered by cached routes.
- Taxonomy names in queries must match the seed's `"name"` field exactly (e.g., `"category"` not `"categories"`).

## This Template

A blog with posts, pages, categories, tags, full-text search, and RSS. Designed for personal writing, technical writing, indie newsletters, and anything where the writing is the product. Industrial-dark magazine aesthetic: a tiled brick-wall backdrop, frosted cards over near-black, Playfair Display headings with Inter and JetBrains Mono labels, and real article structure with bylines and reading time.

## Pages

| Page        | Path               | What it shows                                                                                                                  |
| ----------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| Home        | `/`                | Site title and tagline, featured grid of five flagged posts (centre hero + two stacked either side), Stories feed (compact archive rows on phones)                                       |
| All posts   | `/posts`           | Post rows with thumbnails, excerpts, bylines, and tag chips; cursor pagination                                                 |
| Post detail | `/posts/[slug]`    | Title, excerpt, featured image, then a two-column reading view: body plus a table of contents and sidebar widgets on desktop; tags sit between the body and comments |
| Search      | `/search`          | Full-text search over posts; matching title terms are highlighted                                                              |
| Page        | `/pages/[slug]`    | Title beside a frosted content card on desktop; on smaller screens the title moves inside the card above the body |
| Category    | `/category/[slug]` | Card grid of posts in a category                                                                                               |
| Tag         | `/tag/[slug]`      | Card grid of posts with a tag                                                                                                  |
| RSS         | `/rss.xml`         | Generated feed                                                                                                                 |

## Schema

- `posts` collection: `title`, `featured_image`, `content` (Portable Text), `excerpt` (text).
- `pages` collection: `title`, `content` (Portable Text). Used for `/about` etc.
- Taxonomies: `category`, `tag`.
- Single `primary` menu (Home, News, Reviews, Misc, About by default).
- Categories seeded as `news`, `reviews`, `misc`.

Site settings have `title` and `tagline`. The header and footer use the image logo (`/logo.webp` or the CMS `logo` setting) with the title as alt text; the tagline renders in the home masthead and the footer.

## Visual character

The site is an **industrial dark** theme: a fixed brick-wall texture (`/bg-repeat.webp`) that tiles edge to edge behind the content, under a light theme-independent black wash. Content sits on near-black surfaces (`--color-bg` `#050505`, `--color-surface` `#111111`) and frosted `--color-card` panels with hairline rings. Dark is the default; a light "graphite paper" variant ships for the header/footer theme toggle.

**MalaysianGeek purple** (`#9b1cb6`) is the brand colour and drives buttons, links, active nav, category labels, and focus rings. A **tech red** accent (`--color-accent`, `#fe0001`) is reserved for live/trending/featured badges only -- never for body text or broad surfaces.

Every page sits in the frame: full-width sections (`.band`) share a consistent `.frame` content width. Cards are frosted `--color-card` panels with a hairline ring and a soft shadow; on hover they lift and their image zooms.

Type pairs **Playfair Display** (via `--font-heading`) for headings with **Inter** (`--font-body`) for body and **JetBrains Mono** (`--font-mono`) for code and uppercase `.eyebrow` labels. The homepage is a magazine layout: `FeaturedGrid` (five flagged posts: centre hero + two stacked either side; on phones one tall card with tiny dots that auto-advance every 5s) and `HomeFeed` (Trending/Latest switcher, horizontal `feed` cards, "Trending topics" sidebar).

The article layout is the standout feature: an inline byline/date/reading row under the title at every size, a 680px body column, and a widget rail with the table of contents on desktop. Below 1100px it collapses to one column and the table of contents is hidden; the widgets move under the article. Tags sit between the body and the comments. Don't flatten it on desktop -- the layout signals "this is something to read".

## Customisation

Design tokens live in `src/styles/tokens.css` with their default values. To restyle the site, override tokens in `src/styles/theme.css` -- declarations there are unlayered, so they always beat the `@layer base` defaults. Don't edit `tokens.css` or `Base.astro` for visual changes.

Colours are defined with `light-dark(<light>, <dark>)`, so each token carries both modes. Overriding with a plain colour changes light and dark at once; use `light-dark()` in the override to keep them distinct. There is no separate dark palette to maintain. The brand tokens are `--color-brand` (purple), `--color-brand-hover`, `--color-on-brand` (white), and `--color-brand-ring`; the secondary accent is `--color-accent` plus `--color-accent-soft`, used only for badges.

Webfonts are configured in `astro.config.mjs` under `fonts:`. Body is Inter (`--font-body`), headings are Playfair Display (`--font-heading`), code is JetBrains Mono (`--font-mono`). To swap a face, change the `name:` for the entry bound to that `cssVariable` and keep the `<Font>` tags in `Base.astro` in sync.

`Base.astro` defines the shared building blocks every page uses: `.frame` (content width), `.band` and `.section` (full-width sections), `.intro`, `.eyebrow`, `.chip`, `.btn`, and `.post-grid`. Posts render through `src/components/PostCard.astro` (`card`, `feature`, `feed`, and `row` variants), with `FeaturedGrid.astro` and `HomeFeed.astro` composing the homepage. Bylines render through `src/components/Byline.astro`.

CSS variables worth knowing (see `tokens.css` for the full list):

- `--color-brand`, `--color-brand-hover`, `--color-on-brand`, `--color-brand-ring`
- `--color-accent`, `--color-accent-soft` -- tech-red badge accent
- `--color-bg`, `--color-surface`, `--color-fill`, `--color-text`, `--color-text-secondary`, `--color-muted`, `--color-border`, `--color-ring`
- `--font-body`, `--font-heading`, `--font-mono`
- `--font-weight-heading` (600) / `--font-weight-display` (700) -- heading weights
- `--font-size-display` -- page titles
- `--radius-sm` / `--radius` / `--radius-lg` / `--radius-xl` / `--radius-full`, `--card-padding`
- `--shadow-card`, `--shadow-card-hover`
- `--content-width` (680px) -- article body column
- `--wide-width` (1200px) -- the frame width
- `--frame-margin` / `--frame-padding` -- space outside and inside the frame
- `--gutter-width` (280px) -- the article's widget rail
- `--avatar-size-{xs,sm,md,lg}` -- byline avatar sizes

## What not to do

- Don't use `--color-accent` (tech red) for anything except live/trending/featured badges -- no coloured section backgrounds.
- Don't replace Playfair Display with a novelty display face (Bebas, Anton, etc.). Headings are editorial serif; body stays Inter.
- Don't break the frame: new sections should be `.band` elements with their content in a `.frame`, so the content width lines up.
- Don't collapse the article's widget rail on desktop -- it's part of the reading experience.
- Don't use stock blog copy ("Welcome to my blog", "Stay tuned for more"). Write a real tagline that says what this blog is about.
- Don't seed the home page with three identical placeholder posts. If you only have one real post, show one real post.
- Comments are enabled on posts and rendered on the post detail page. Configure moderation before publishing the site, or remove `commentsEnabled` and the comments UI together.
