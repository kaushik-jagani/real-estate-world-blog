# Real Estate World — Project Reference for Claude

> **READ THIS FILE BEFORE EVERY IMPLEMENTATION.**  
> This document is the single source of truth for project structure, patterns, rules, and conventions.

---

## 1. Project Overview

**Site name:** Real Estate World  
**Domain:** realestateworld.blog (to be configured)  
**Purpose:** Global real estate blog — property market insights, trends, buying guides, and analysis for cities worldwide.  
**Category:** Only "Real Estate" — no category switcher anywhere.  
**Scope:** We are a **blog only** — we do NOT sell, buy, or broker properties.  
**Analytics:** Google Analytics `G-01LZXPD19D` on EVERY HTML page.  
**AdSense:** Fully compliant layout — ad slots marked with `<!-- AD SLOT -->` comments only.

---

## 2. Tech Stack

- Pure HTML5 / CSS3 / Vanilla JavaScript (ES5-compatible)
- No frameworks, no build tools, no npm required
- Data driven: every post is a `post.json` file fetched at runtime
- Central index: `data/posts/index.json` — flat array of post metadata (single fetch, scales to 11k+)
- Fonts: Google Fonts — Inter (body) + Playfair Display (headings)
- Accent color: `#0EA5E9` (sky blue)

---

## 3. Folder Structure

```
/
├── index.html                        ← Home landing page (hero + features + recent posts)
├── .htaccess                         ← Apache clean URL routing
├── claude.md                         ← THIS FILE (read before any work)
├── docs/                             ← Project documentation
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

**Important:** Always edit files in `pages/` then sync copies to clean URL folders.

---

## 4. URL Structure & Navigation

| Page | Clean URL | Source file | Nav item |
|---|---|---|---|
| Home | `/` | `/index.html` | Home (active) |
| Blog listing | `/blog` | `/pages/blog-list.html` | Real Estate (active) |
| Blog post | `/blog/[slug]` | `/pages/blog-post.html` | Real Estate (active) |
| About | `/about` | `/pages/about.html` | About (active) |
| Contact | `/contact` | `/pages/contact.html` | Contact (active) |
| Privacy Policy | `/privacy-policy` | `/pages/privacy-policy.html` | — |
| Disclaimer | `/disclaimer` | `/pages/disclaimer.html` | — |

**Logo click** → `/` (home)  
**Never expose** `/pages/blog-post.html?slug=` in visible UI links.

For local dev with Live Server: clean URL folders (`blog/`, `about/`, etc.) contain `index.html` copies.

---

## 5. Central Index — `data/posts/index.json`

This is the **single source of truth** for all post listings. One fetch serves the home page, blog list, and sidebar.

```json
[
  {
    "slug": "luxury-apartments-dubai-guide",
    "title": "Luxury Apartments in Dubai: Smart Guide for Modern Buyers (2026)",
    "author": "Admin",
    "date": "2026-04-18",
    "readTime": "8 min read",
    "category": "Real Estate",
    "excerpt": "Dubai has become one of the top cities...",
    "coverImage": "/assets/images/posts/luxury-apartments-dubai-guide/cover.jpg",
    "featured": true
  }
]
```

**Adding a new entry:** Append a new object to this array. Fields: `slug`, `title`, `author`, `date`, `readTime`, `category`, `excerpt`, `coverImage`, `featured` (boolean).

---

## 6. Post JSON Format

Every post lives at `data/posts/[slug]/post.json`. Full schema:

```json
{
  "slug": "post-slug-here",
  "url": "https://realestateworld.blog/blog/post-slug-here",
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
    "url": "https://realestateworld.blog/blog/slug",
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

### Section types rendered by render.js:
| Property | Rendered as |
|---|---|
| `content` | `<p>` paragraph |
| `list` | `<ul>` bullet list |
| `subsections[]` | `<h3>` + content or list per item |
| `faqs[]` | Collapsible accordion items |
| `table` | Striped scrollable `<table>` |
| `quote` | Large stylised blockquote |
| `callout` | Coloured callout box (info/tip/warning) |
| `image` | Full-width rounded `<img>` (lazy) |

---

## 7. Adding a New Blog Post (Step-by-Step)

### Quick version (2 minutes):
1. Drop `post.json` into `data/posts/[slug]/`
2. Add entry to `data/posts/index.json`
3. Create `blog/[slug]/index.html` (copy of `pages/blog-post.html`)
4. Optionally add cover image to `assets/images/posts/[slug]/cover.jpg`
5. Done — visit `/blog/[slug]`

### Detailed steps:
1. **Receive `post.json`** from user/author
2. **Create folder:** `data/posts/[slug]/`
3. **Place `post.json`** in that folder
4. **Add to index.json:** Append a new object to the array in `data/posts/index.json` with: slug, title, author, date, readTime, category, excerpt, coverImage, featured
5. **Cover image:** Either use an Unsplash URL in `coverImage` field, or create `assets/images/posts/[slug]/cover.jpg`
6. **Create clean URL folder:** `blog/[slug]/index.html` — copy of `pages/blog-post.html`
7. **Test:** Visit `/blog/[slug]`

### What you do NOT need to do:
- No code changes to any JS/CSS/HTML files
- No rebuilds, no deployments (static site)
- The blog-list page auto-discovers new posts from `index.json`
- The sidebar auto-shows other posts from `index.json`

---

## 8. Syncing Clean URL Copies

After editing any file in `pages/`, sync copies:

```powershell
Copy-Item "pages\about.html" "about\index.html" -Force
Copy-Item "pages\contact.html" "contact\index.html" -Force
Copy-Item "pages\privacy-policy.html" "privacy-policy\index.html" -Force
Copy-Item "pages\disclaimer.html" "disclaimer\index.html" -Force
Copy-Item "pages\blog-list.html" "blog\index.html" -Force
Copy-Item "pages\blog-post.html" "blog\[slug]\index.html" -Force
```

---

## 9. All Known Post Slugs

Update this list whenever a new post is added:

```
luxury-apartments-dubai-guide
```

---

## 10. Image Handling

- Cover images: `assets/images/posts/[slug]/cover.jpg`
- In `post.json`, `coverImage` can be absolute URL or local path starting with `/`
- All `<img>` use `loading="lazy"`
- Always include meaningful `alt` attributes

---

## 11. Sidebar — "Trending Reads"

The right sidebar on blog post pages uses data from `index.json` (no extra fetches).

CSS: `position: sticky; top: var(--header-height); height: calc(100vh - var(--header-height));`

On mobile (≤1024px): sidebar becomes static, full-width below article.

---

## 12. Google Analytics

Tag ID: `G-01LZXPD19D` — must be in `<head>` of **every** HTML page.

---

## 13. Google AdSense Rules

- Ad slots marked with `<!-- AD SLOT -->` comments only
- Positions: below header, between article sections, below "You May Also Like"
- No pop-ups, no interstitials, no auto-playing video
- All images have `alt` text, proper meta tags on every page
- Privacy Policy and Disclaimer pages are AdSense-compliant

---

## 14. Dark / Light Mode

- Toggle: moon icon (light) / sun icon (dark)
- `data-theme="dark"` on `<html>`
- `localStorage` key: `pi-theme`
- `theme.js` loaded **once** in `<head>` (synchronous, not deferred) — never load twice!
- Dark bg: `#0f0f1a` | Light bg: `#ffffff`

---

## 15. Responsive Breakpoints

| Breakpoint | Layout |
|---|---|
| ≥1200px | Full desktop: 3-column post layout (TOC + content + sidebar) |
| 1024px–1199px | Sidebar drops below; 2-col post grid |
| 768px–1023px | 2-col grid; TOC → accordion |
| 480px–767px | 1-col grid |
| 360px–479px | Minimum supported |

---

## 16. Development Server

```bash
# VS Code Live Server (recommended)
# Or:
npx serve . -p 3000
python -m http.server 3000
```

Visit: `http://localhost:5500/` (Live Server) or `http://localhost:3000/`

---

## 17. Key Conventions

- **Edit source files in `pages/`**, then copy to clean URL folders
- **Use absolute paths** (`/assets/...`) in `blog-post.html` (works at any URL depth)
- **Use relative paths** (`../assets/...`) in other `pages/*.html` files
- **Never load `theme.js` twice** in any page (causes double-toggle = no effect)
- **Use plain apostrophes** in JSON, not escaped `\'`
- **All nav links:** Home → `/`, Real Estate → `/blog`, About → `/about`, Contact → `/contact`
- **Logo always links to** `/`

---

*Last updated: April 18, 2026*
