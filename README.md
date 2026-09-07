# vzero-blog

A solo-author static blog built with Astro, Sveltia CMS, Pagefind search and Giscus
comments — the fourth in a set of independent sibling blogs, each with its own visual
design and its own content:

- GitHub Pages blog — warm serif, rounded cards
- GitLab Pages blog — same family as above, independent content
- Cloudflare Pages blog — cool, flat, technical, monospace labels
- **vzero-blog (this project)** — bold, high-contrast, thick borders and hard offset
  shadows; a print-zine/neo-brutalist register

**Live at https://creativedigitalgrowth.vercel.app/** — deployed via v0.app -> Vercel,
connected to [CreativeDigitalGrowth/vzero-blog](https://github.com/CreativeDigitalGrowth/vzero-blog)
on GitHub (`main`, auto-deploys on push). See [CLAUDE.md](CLAUDE.md) for what's still
left to configure (Giscus, socials, author details).

## Quick start

```bash
npm install
npm install --no-save --force @astrojs/compiler-binding-wasm32-wasi  # Windows only, see CLAUDE.md
npm run dev
```

Open http://localhost:4321/ for the site, or
http://localhost:4321/admin/index.html for the CMS (choose **"Work with Local
Repository"** — no account needed yet, see CLAUDE.md).

## Features

- Content collections with a Zod-validated frontmatter schema
- Sveltia CMS at `/admin/`, GitHub-backed (or local-backend for offline editing)
- Categories and tags, each with their own paginated archive
- Full-text search (Pagefind) — works against `npm run preview`, not `npm run dev`
- Giscus comments (kept unconfigured until GitHub Discussions is enabled on the repo)
- RSS feed, sitemap, per-post JSON-LD, light/dark theme toggle
- Optional About / Search / Contact pages, gated behind flags in `src/consts.ts`
- Optional Google Maps embed per post
