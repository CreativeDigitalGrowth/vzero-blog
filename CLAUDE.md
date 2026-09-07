# Working in this repository

A solo-author static blog: Astro 7 + TypeScript, Sveltia CMS, Pagefind search, Giscus
comments — the same proven scaffold as the sibling GitHub Pages, GitLab Pages and
Cloudflare Pages blogs, but with its own codebase, content and visual design going
forward.

**Deployed** at https://creativedigitalgrowth.vercel.app/ via v0.app -> Vercel, connected
to `CreativeDigitalGrowth/vzero-blog` on GitHub (`main`, auto-deploys on push). One item
still open — see "Still to configure" below.

## Design language

Bold, high-contrast, tactile: thick black borders, hard offset "sticker" shadows
(no blur), saturated flat colour chips, a condensed grotesk display font. Deliberately
different from both siblings — not the warm serif/rounded-card treatment of the
GitHub/GitLab blogs, and not the cool flat hairline-technical/monospace treatment of the
Cloudflare blog. The whole language lives in `src/styles/global.css`; components carry
the same class names as the sibling projects (`.card`, `.pill`, `.button`, `.toc`, …) so
the visual layer can be re-skinned again later without touching component logic.

## Development

```bash
npm run dev      # localhost:4321/ — drafts visible
npm run build    # production build + Pagefind index
npm run preview  # serves dist/ — the only faithful test of search and base paths
npm run check    # TypeScript + Astro diagnostics; keep this at 0 errors
```

**On this Windows machine**, Smart App Control blocks Astro's native compiler binary.
After every `npm install` or `npm ci`:

```bash
npm install --no-save --force @astrojs/compiler-binding-wasm32-wasi
```

## Editing content

The CMS at `/admin/` on the live site uses the GitHub backend — **"Sign In Using Access
Token"** with a fine-grained PAT scoped to `CreativeDigitalGrowth/vzero-blog` (same
pattern as the sibling Cloudflare blog; **not** "Sign In with GitHub", which hangs — see
that project's `docs/troubleshooting.md`). Saving is a commit to `main`, which Vercel
picks up automatically.

Locally, `local_backend: true` (set in `public/admin/config.yml`) is also still
available — it lets Sveltia read and write this working copy directly through the
browser's File System Access API (Chromium-based browsers only), useful for editing
without every save reaching the live site immediately:

```bash
npm run dev
```

Open **http://localhost:4321/admin/index.html** (the explicit filename is required in
dev) and choose **"Work with Local Repository"**.

## Rules that are easy to get wrong

(Same rules as the sibling blogs — carried over unchanged because the underlying
scaffold is unchanged, only the visual layer differs.)

**Never write a root-absolute internal path.** Use the helpers in `src/lib/url.ts`:

| Helper | For |
| --- | --- |
| `withBase(p)` | paths you author — `/about/` |
| `absFromBuiltPath(p, site)` | paths Astro produced (`Astro.url.pathname`, `ImageMetadata.src`, `paginate()` URLs) — already based |
| `absUrl(p, site)` | absolute URL from a path you author |

**`paginate()` URLs already include the base.** `Pagination.astro` takes them raw.

**Query posts through `getPosts()`** in `src/lib/posts.ts`, never `getCollection`
directly — that is where drafts are filtered and date ordering happens.

**Frontmatter image paths are relative to the Markdown file** —
`../../assets/images/uploads/…`. `media_folder`/`public_folder` in
`public/admin/config.yml` must stay in sync with wherever posts live.

**Site-wide settings live in `src/consts.ts` and nowhere else.**

**The CMS schema and the Zod schema must match.** `public/admin/config.yml` field names
and `src/content.config.ts` are one contract.

**`public/admin/config.yml` is YAML.** Quote any string containing `: `.

## Still to configure

- `src/consts.ts` — `GISCUS.repo` (empty until GitHub Discussions is enabled on
  `CreativeDigitalGrowth/vzero-blog` — needs repo admin, not just push access),
  `SOCIAL_LINKS` (empty), `AUTHOR_NAME`/`AUTHOR_BIO`/`AUTHOR_EMAIL` (still template
  defaults)
- A custom domain, if `creativedigitalgrowth.vercel.app` is not the permanent home —
  update it together in `astro.config.mjs`, `public/admin/config.yml`
  (`site_url`/`display_url`) and `public/robots.txt` in one pass, same as any Vercel
  custom-domain switch.

## Before calling a change done

```bash
npm run check    # expect 0 errors
npm run build
```

If the change is visible in a browser, verify with `npm run preview` rather than
`npm run dev` — search, `/admin/` and draft exclusion all behave differently between the
two.

## Documentation

https://docs.astro.build — [Routing](https://docs.astro.build/en/guides/routing/),
[Content collections](https://docs.astro.build/en/guides/content-collections/),
[Images](https://docs.astro.build/en/guides/images/).
