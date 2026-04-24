# Real Estate World — CLAUDE.md

> **READ THIS FILE BEFORE EVERY IMPLEMENTATION.**
> Single source of truth for structure, patterns, rules, and conventions.

---

## Workflow Orchestration

### 1. Plan Mode Default
- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions)
- If something goes sideways, STOP and re-plan immediately
- Use plan mode for verification steps, not just building
- Write detailed specs upfront before touching any file

### 2. Before Every Implementation
- Read this file fully before starting any task
- Always edit source files in `pages/` — then sync copies to clean URL folders
- Never expose `/pages/blog-post.html?slug=` in visible UI links
- Never load `theme.js` twice in any page — causes double-toggle with no effect

### 3. Verification Before Done
- Never mark a task complete without proving it works
- Ask yourself: "Would a senior dev approve this?"
- Run tests, check logs, demonstrate correctness
- Diff behavior between main and your changes when relevant

### 4. Demand Elegance (Balanced)
- For non-trivial changes: pause and ask "is there a more elegant way?"
- If a fix feels hacky: implement the elegant solution instead
- Skip this for simple, obvious fixes — don't over-engineer
- Challenge your own work before presenting it

### 5. Autonomous Bug Fixing
- When given a bug report: just fix it — don't ask for hand-holding
- Point at logs, errors, failing tests — then resolve them
- Zero context switching required from the user
- Go fix failing issues without being told exactly how

### 6. Self-Improvement Loop
- After ANY correction: note the pattern to avoid repeating it
- Write rules for yourself that prevent the same mistake
- Ruthlessly iterate until mistake rate drops

---

## Task Management

1. **Plan First**: Write plan with checkable steps before starting
2. **Verify Plan**: Confirm approach before implementation
3. **Track Progress**: Mark items complete as you go
4. **Explain Changes**: High-level summary at each step
5. **Document Results**: Note what was done and why
6. **Capture Lessons**: Record corrections to avoid repeat mistakes

---

## Core Principles

- **Simplicity First**: Make every change as simple as possible. Impact minimal code.
- **No Laziness**: Find root causes. No temporary fixes. Senior developer standards.
- **Minimal Impact**: Only touch what's necessary. No side effects, no new bugs.
- **Data-Driven**: Every post is a `post.json`. `data/posts/index.json` is the single source of truth — never duplicate data.
- **No Frameworks**: Pure HTML5 / CSS3 / Vanilla JS only. No npm, no build tools.

---

## Project Identity

- **Site name:** Real Estate World
- **Domain:** realestate.globalinfonest.com (Netlify)
- **GitHub repo:** `kaushik-jagani/real-estate-world-blog` (private)
- **Purpose:** Global real estate blog — insights, trends, buying guides
- **Scope:** Blog only — we do NOT sell, buy, or broker properties
- **Category:** "Real Estate" only — no category switcher anywhere
- **Footer:** © 2026 GlobalInfoNest
- **Analytics:** Google Analytics `G-F9TKTH7W32` — on EVERY HTML page
- **AdSense:** Ad slots hidden by default (`display: none`) until real ads are placed
- **Accent color:** `#0EA5E9` (sky blue)
- **Fonts:** Inter (body) + Playfair Display (headings) via Google Fonts

---

## Folder Structure

```
/
├── index.html                        ← Home landing page (hero + recent posts)
├── .htaccess                         ← Apache clean URL routing
├── _redirects                        ← Netlify rewrite rules (dynamic blog routing)
├── claude.md                         ← THIS FILE (read before any work)
├── docs/
│   └── PROJECT-GUIDE.md              ← Full dev guide, adding blogs, architecture
├── assets/
│   ├── css/
│   │   ├── base.css                  ← Reset, CSS variables, typography
│   │   ├── header.css                ← Sticky header, nav, search, hamburger, theme toggle
│   │   ├── article.css               ← Post page layout, TOC, content blocks (3-col grid)
│   │   ├── sidebar.css               ← YouTube-like sidebar, post cards
│   │   ├── blog-list.css             ← Blog listing grid, hero, featured card, post cards
│   │   └── dark-mode.css             ← Dark theme overrides via [data-theme="dark"]
│   ├── js/
│   │   ├── theme.js                  ← Dark/light toggle + localStorage (loaded in <head>)
│   │   ├── render.js                 ← renderPost(json) — renders all section types
│   │   ├── router.js                 ← Slug routing, sidebar, "You May Also Like"
│   │   └── header.js                 ← Hamburger menu, search toggle, drawer
│   ├── icons/
│   │   └── favicon.svg
│   └── images/
│       └── posts/
│           └── [slug]/               ← One folder per post
│               └── cover.jpg         ← Main cover image
├── data/
│   ├── site-config.json              ← Global site settings
│   └── posts/
│       ├── index.json                ← ⭐ CENTRAL INDEX — flat array of all post metadata
│       └── [slug]/
│           └── post.json             ← Full post content: meta + OG + sections
├── pages/                            ← Source HTML files (edit these)
│   ├── blog-list.html                ← /blog — all posts grid + hero + pagination
│   ├── blog-post.html                ← /blog/[slug] — article page
│   ├── about.html                    ← /about
│   ├── contact.html                  ← /contact
│   ├── disclaimer.html               ← /disclaimer
│   └── privacy-policy.html           ← /privacy-policy
├── blog/                             ← Clean URL copies (synced from pages/)
│   ├── index.html                    ← Copy of blog-list.html
│   └── [slug]/
│       └── index.html                ← Copy of blog-post.html
├── about/index.html                  ← Copy of pages/about.html
├── contact/index.html                ← Copy of pages/contact.html
├── disclaimer/index.html             ← Copy of pages/disclaimer.html
├── privacy-policy/index.html         ← Copy of pages/privacy-policy.html
└── components/
    ├── header.html                   ← Reference snippet
    ├── footer.html                   ← Reference snippet
    └── post-card.html                ← Reference snippet
```

---

## URL Structure & Navigation

| Page | Clean URL | Source file | Nav item |
|---|---|---|---|
| Home | `/` | `/index.html` | Home (active) |
| Blog listing | `/blog` | `/pages/blog-list.html` | Real Estate (active) |
| Blog post | `/blog/[slug]` | `/pages/blog-post.html` | Real Estate (active) |
| About | `/about` | `/pages/about.html` | About (active) |
| Contact | `/contact` | `/pages/contact.html` | Contact (active) |
| Privacy Policy | `/privacy-policy` | `/pages/privacy-policy.html` | — |
| Disclaimer | `/disclaimer` | `/pages/disclaimer.html` | — |

---

## Adding a New Blog Post

### Quick Steps (2 minutes):
1. Drop `post.json` into `data/posts/[slug]/`
2. Append entry to `data/posts/index.json`
3. Netlify: done — `_redirects` handles routing automatically
4. Local dev only: create `blog/[slug]/index.html` (copy of `pages/blog-post.html`)
5. Optionally add cover image to `assets/images/posts/[slug]/cover.jpg`

### index.json Entry Format:
```json
{
  "id": 5,
  "slug": "your-post-slug",
  "title": "Post Title",
  "author": "Author Name",
  "date": "2026-04-19",
  "readTime": "8 min read",
  "category": "Real Estate",
  "excerpt": "Short excerpt here...",
  "coverImage": "/assets/images/posts/your-post-slug/cover.jpg",
  "featured": false
}
```

### Sort Order:
- By `date` descending → then by `id` descending (tiebreaker for same-day posts)
- Next post ID = **5**

### What You Do NOT Need to Do:
- No JS/CSS/HTML changes
- No rebuilds or manual deployments
- Blog list and sidebar auto-discover new posts from `index.json`

---

## Post JSON Schema

Every post lives at `data/posts/[slug]/post.json`:

```json
{
  "slug": "post-slug-here",
  "url": "https://realestate.globalinfonest.com/blog/post-slug-here",
  "title": "Post Title",
  "author": "Author Name",
  "date": "2026-04-18",
  "readTime": "8 min read",
  "category": "Real Estate",
  "coverImage": "/assets/images/posts/post-slug-here/cover.jpg",

  "meta": {
    "title": "SEO Title",
    "description": "Meta description",
    "keywords": ["keyword1", "keyword2"],
    "robots": "index, follow"
  },

  "openGraph": {
    "type": "article",
    "title": "OG Title",
    "description": "OG Description",
    "url": "https://realestate.globalinfonest.com/blog/slug",
    "site_name": "Real Estate World",
    "image": "https://...",
    "locale": "en_US"
  },

  "twitter": {
    "card": "summary_large_image",
    "title": "Twitter Title",
    "description": "Twitter Description",
    "image": "https://..."
  },

  "sections": [
    { "heading": "Section Title", "content": "Paragraph text here." },
    { "heading": "Section Title", "list": ["item 1", "item 2"] },
    {
      "heading": "Section With Subsections",
      "subsections": [
        { "title": "Sub Title", "content": "Sub paragraph." },
        { "title": "Sub Title", "list": ["item 1"] }
      ]
    },
    {
      "heading": "FAQs",
      "faqs": [
        { "question": "Q text?", "answer": "A text." }
      ]
    }
  ]
}
```

### Section Types (rendered by render.js):

| Property       | Renders As |
|----------------|------------|
| `content`      | `<p>` paragraph |
| `list`         | `<ul>` bullet list |
| `subsections[]`| `<h3>` + content or list per item |
| `faqs[]`       | Collapsible accordion |
| `table`        | Striped scrollable `<table>` |
| `quote`        | Large stylised blockquote |
| `callout`      | Coloured callout box (info/tip/warning) |
| `image`        | Full-width rounded `<img>` (lazy) |

---

## Known Post Slugs

| ID | Slug |
|----|------|
| 1  | luxury-apartments-dubai-guide |
| 2  | luxury-apartments-usa-guide |
| 3  | luxury-apartments-monaco-guide |
| 4  | luxury-apartments-switzerland-guide |

---

## Key Conventions

- **Paths:** Use absolute paths (`/assets/...`) in `blog-post.html`; relative (`../assets/...`) in other `pages/` files
- **Apostrophes:** Plain `'` in JSON — not escaped `\'`
- **Nav links:** Home → `/`, Real Estate → `/blog`, About → `/about`, Contact → `/contact`
- **Logo:** Always links to `/`
- **Search:** Hidden by default — only shown on icon click via `.open` class
- **Hamburger:** Uses `#hamburger-btn` selector (NOT `.hamburger-btn`)
- **Ad slots:** Always `display: none` until real ads are placed
- **`theme.js`:** Load ONCE in `<head>`, synchronous, never deferred
- **Images:** All `<img>` use `loading="lazy"` with meaningful `alt` attributes
- **Cover images:** `coverImage` can be absolute URL or local path starting with `/`

---

## Dark / Light Mode

- Toggle: moon icon (light) / sun icon (dark)
- `data-theme="dark"` on `<html>`
- `localStorage` key: `pi-theme`
- Dark bg: `#0f0f1a` | Light bg: `#ffffff`

---

## Syncing Clean URL Copies

After editing any file in `pages/`, run:

```powershell
Copy-Item "pages\about.html" "about\index.html" -Force
Copy-Item "pages\contact.html" "contact\index.html" -Force
Copy-Item "pages\privacy-policy.html" "privacy-policy\index.html" -Force
Copy-Item "pages\disclaimer.html" "disclaimer\index.html" -Force
Copy-Item "pages\blog-list.html" "blog\index.html" -Force
```

---

## Responsive Breakpoints

| Breakpoint   | Layout |
|--------------|--------|
| ≥1200px      | 3-column: TOC + content + sidebar |
| 1024–1199px  | Sidebar drops below; 2-col post grid |
| 768–1023px   | 2-col grid; TOC → accordion |
| 480–767px    | 1-col grid |
| 360–479px    | Minimum supported |

---

## Mobile / Responsive Rules

- **Search input:** Hidden by default (`width: 0; opacity: 0; pointer-events: none`). Revealed via `.search-input-wrapper.open` class on toggle click.
- **Hamburger button:** Uses `#hamburger-btn` selector (NOT `.hamburger-btn`). Hidden on desktop, shown via `display: flex` at ≤1024px.
- **Ad slots:** All ad slot classes have `display: none` until real ads are placed.
- **Sidebar:** Sticky on desktop; becomes static full-width below article on mobile (≤1024px).
- **Rule:** Never let elements with `position: absolute` be visible on mobile without explicit size constraints.

---

## AdSense Compliance

- Ad slots marked with `<!-- AD SLOT -->` comments only
- Positions: below header, between article sections, below "You May Also Like"
- No pop-ups, no interstitials, no auto-playing video
- Privacy Policy and Disclaimer pages are AdSense-compliant

---

## Netlify Deployment

- **`_redirects` rule:** `/blog/*  /pages/blog-post.html  200`
- Physical files take priority: `blog/index.html` still serves `/blog/`
- No need to create `blog/[slug]/index.html` on Netlify — local dev only

---

## Development Server

```bash
# VS Code Live Server (recommended)
# Or:
npx serve . -p 3000
python -m http.server 3000
```

Visit: `http://localhost:5500/` (Live Server) or `http://localhost:3000/`

---

*Last updated: April 19, 2026*
