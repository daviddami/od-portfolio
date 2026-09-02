# Technical Report — Personal Portfolio Website

**Project:** Personal portfolio for a future Computer Engineer
**Author:** Olokoba David Oluwadamilola
**Date:** September 2026
**Live site:** https://YOUR-USERNAME.github.io/portfolio/ *(replace after deployment)*
**Repository:** https://github.com/YOUR-USERNAME/portfolio

---

## 1. Overview

This report accompanies a multi-page static portfolio built with XHTML-compatible HTML5, CSS3 and JSON only. No JavaScript, no CMS and no static-site generator were used. The site contains eight linked pages — Home, About Me, Educational Background, Technical Skills, Projects, Hobbies & Interests, Curriculum Vitae and Contact Me — sharing one stylesheet and one content data file.

### 1.1 Directory structure

```
portfolio/
├── index.html            Home
├── about.html            About Me
├── education.html        Educational Background
├── skills.html           Technical Skills
├── projects.html         Projects (CSS :target modals)
├── hobbies.html          Hobbies and Interests
├── cv.html               Curriculum Vitae (print-ready)
├── contact.html          Contact Me (mailto: form)
├── assets/
│   ├── css/style.css     Single stylesheet, mobile-first
│   └── images/           SVG portrait, project thumbnails, favicon
├── data/
│   └── data.json         Content model for every entity on the site
├── docs/
│   └── technical-report.md
├── .nojekyll             Tells GitHub Pages to serve files as-is
└── README.md
```

Assets are separated by type (`/assets/css/`, `/assets/images/`) and data is isolated in `/data/`, so any file can be located from its role alone and nothing needs to move if a page is added.

### 1.2 Standards compliance

Every page was checked with the Nu HTML Checker (the engine behind validator.w3.org) and the W3C CSS validator; all pages and the stylesheet return zero errors. Each page is additionally well-formed XML (verified with `xmllint`), meeting the XHTML syntax rules: lowercase tags, quoted attributes, closed void elements (`<meta … />`, `<img … />`, `<input … />`) and a declared XHTML namespace on the root element.

---

## 2. Visual design rationale

### 2.1 Concept

The site follows a warm, editorial portfolio style built around the author's own portrait: a stone-grey surface, a condensed uppercase headline ("A COMPUTER *& Creative* ENGINEER") whose middle line switches to an italic serif, and serif body copy. The portrait sits in an arch-shaped frame whose tan background is drawn from the photograph itself, so the image and the page share one palette. The home page reads top to bottom as: hero with stats → four-column services strip → "Worked with" grid → tools marquee → manifesto → portfolio with a full-width feature and caption rows → scroll-snap photo strip → large closing call to action.

### 2.2 Colour and contrast

| Token        | Hex       | Role                                                    |
|--------------|-----------|---------------------------------------------------------|
| `--bg`       | `#D9D8D2` | Page background (light theme)                           |
| `--surface`  | `#E6E5DF` | Menu overlay, cards, modal                              |
| `--ink`      | `#171614` | Headings, pills, brand                                  |
| `--text`     | `#2E2C29` | Body copy                                               |
| `--muted`    | `#6E6B65` | Captions and secondary text                             |
| `--accent`   | `#8A5A3C` | Warm brown accent: dates, marquee dots, hover states    |
| `--photo-bg` | `#D6A377` | Arch behind the portrait, sampled from the photograph   |

Ink on the background reaches 13.2:1, body text 9.6:1, muted grey 4.6:1 and the accent 5.0:1 — all at or above WCAG AA. A dark theme swaps the same tokens (`#1B1A18` background, `#F0EEE8` ink, `#C9946B` accent) and keeps every pairing above 4.5:1.

### 2.3 Typography

Four families, each with one job: **Anton** (condensed) for the big headings, **Instrument Serif** italic for the single emphasised word inside them, **Oswald** for small tracked labels (stats, captions, pills, navigation index numbers) and **Newsreader** for serif body copy. Fonts load from Google Fonts through a CSS `@import` with system fallbacks. Headings use 0.98 line-height so stacked lines read as one block; body copy sits at 1.05 rem / 1.6 with a 58-character measure.

### 2.4 Layout and responsiveness

Mobile-first, with `min-width` breakpoints at 40, 48, 52, 60 and 64 rem. CSS Grid drives the three-column hero (text / portrait / stats), the four-column services strip, the bordered "Worked with" and skills grids, the manifesto split and the three-up work grid; Flexbox handles the header, pills, caption rows and the horizontal photo strip. Hairlines and bordered cells replace boxes and shadows; the only rounded shapes are the arch, pills and image corners, so they stand out.

### 2.5 CSS-only interactivity and features

| Feature                          | Technique                                                          |
|----------------------------------|--------------------------------------------------------------------|
| Full-width menu overlay ("Menu" pill) | Hidden checkbox + `:checked ~ .site-nav` expanding a numbered grid of links |
| Dark / light theme switch        | A checkbox placed before the `.page` wrapper; `:checked ~ .page` redefines the colour custom properties, so every component re-themes with no script |
| Tools marquee                    | Duplicated list animated with `@keyframes` (`translateX(-50%)`); pauses on hover; disabled under `prefers-reduced-motion` |
| Portfolio hover reveal           | Image scale and a "View project →" pill on `:hover` / `:focus-visible` |
| Photo strip                      | `scroll-snap-type: x mandatory` so each frame snaps into place on swipe |
| "Worked with" role reveal        | Role text fades in on `:hover` / `:focus-within`                   |
| Project detail overlays          | `.modal:target` shows the overlay whose `id` matches the URL fragment |
| Accordions                       | Native `<details>` / `<summary>` styled with `details[open]`        |
| Back-to-top button               | Fixed anchor to `#main` with `scroll-behavior: smooth`             |
| Contact form validation          | HTML5 `required` / `type="email"` with `:invalid` styling           |
| Printable CV                     | `@media print` hides chrome and resets colours                     |

### 2.6 Accessibility

Semantic landmarks, a skip link, `aria-current="page"` in the menu, labelled toggles, `aria-hidden` on decorative icons, visible focus rings on every control including the checkbox-driven labels, motion disabled under `prefers-reduced-motion`, and descriptive `alt` text on every image.

---

## 3. JSON data structure definitions

`data/data.json` is the single content model for the site. Because the implementation cannot execute JavaScript, the file is not fetched at runtime; instead it acts as the authoritative source from which the HTML was authored, and it is the artefact a future build step or CMS would consume. Every entity has a stable string `id` so that records can be cross-referenced (for example, `site.author_id` points to `profile.id`, and project cards link to `projects.html#proj-001`).

### 3.1 Top-level schema

| Key              | Type   | Purpose                                              |
|------------------|--------|------------------------------------------------------|
| `site`           | object | Title, base URL, language, last-updated date, copyright year |
| `profile`        | object | Person entity rendered on Home, About and in the footer |
| `education`      | array  | Institutions and qualifications, chronological        |
| `experience`     | array  | Volunteer and internship roles with duties           |
| `organisations`  | array  | Bodies studied at / worked with (`id`, `name`, `relationship`) |
| `services`       | array  | The four service cards on the home page              |
| `stats`          | array  | Hero statistics (`value`, `label`)                   |
| `tools`          | array  | Strings rendered in the marquee                      |
| `gallery`        | array  | Photo-strip entries (`title`, `date`, `image_url`)   |
| `services`       | array  | Four-column services row (`id`, `title`, `description`) |
| `organisations`  | array  | Names shown in the marquee                           |
| `gallery`        | array  | Photo strip frames (`id`, `image_url`, `caption`)    |
| `soft_skills`    | array  | Core strengths listed on the CV                      |
| `skills`         | array  | Skill groups, each containing rated skill items       |
| `projects`       | array  | Portfolio artefacts                                  |
| `hobbies`        | array  | Interests                                            |
| `contact`        | object | Contact channels and availability                    |

### 3.2 Entity definitions

**Project** (`projects[]`)

| Field          | Type            | Constraints                                    |
|----------------|-----------------|------------------------------------------------|
| `id`           | string          | Unique, pattern `proj-NNN`; used as HTML anchor |
| `title`        | string          | Required                                       |
| `date`         | string          | ISO 8601 month, `YYYY-MM`                      |
| `category`     | string          | One of: Events and AV, Creative media, Data and reporting, Web |
| `description`  | string          | Full description; first sentence used on cards |
| `technologies` | array of string | Rendered as tags                               |
| `image_url`    | string          | Relative path under `assets/images/`           |
| `repo_url`     | string (URL)    | Absolute URL to a repository or published work |
| `role`         | string          | Author's contribution                          |

**Education record** (`education[]`): `id`, `institution`, `qualification`, `start_date`, `end_date` (ISO month), `status` (`In progress` \| `Completed`), `location`, `description`, `highlights[]`.

**Experience record** (`experience[]`): `id`, `role`, `organisation`, `location`, `start_date`, `end_date`, `duties[]` — rendered on the CV page.

**Skill group** (`skills[]`): `id`, `category`, `items[]` where each item is `{ name, level (integer 1–4), level_label }`. The integer `level` maps directly to the CSS classes `.level-1` … `.level-4` that set bar width.

**Profile** (`profile`): `id`, `name` (full name in the order last, first, middle), `first_name`, `last_name`, `middle_name`, `headline`, `tagline`, `location`, `email`, `phone`, `image_url`, `summary`, `social[]` of `{ platform, url }`.

**Hobby** (`hobbies[]`): `id`, `title`, `description`.

### 3.3 JSON-LD structured data

Each page embeds a `<script type="application/ld+json">` block in `<head>`. This is declarative metadata, not executable code — browsers never run it — so it complies with the no-JavaScript constraint while giving search engines a machine-readable description of the page. The vocabulary is Schema.org:

| Page          | Schema.org type(s)                                              |
|---------------|-----------------------------------------------------------------|
| index.html    | `ProfilePage` whose `mainEntity` is a `Person`                  |
| about.html    | `AboutPage` → `Person`                                          |
| education.html| `ItemList` of `EducationalOccupationalCredential`               |
| skills.html   | `ItemList` of skill `ListItem`s                                 |
| projects.html | `ItemList` of `CreativeWork` (with `codeRepository`, `dateCreated`, `keywords`) |
| hobbies.html  | `WebPage` with `about` → `Thing[]`                              |
| cv.html       | `WebPage` → `Person`                                            |
| contact.html  | `ContactPage` → `Person` with `ContactPoint`                    |

The `Person` entity uses `sameAs` to link the LinkedIn, Instagram and GitHub profiles, which lets search engines merge the identity across sites. The JSON-LD is derived from the same `data.json` records, so the two representations stay consistent.

---

## 4. HTTP/HTTPS protocol and MIME-type overview

### 4.1 How the static assets are served

When a visitor opens the site, the browser resolves the host name through DNS, opens a TCP connection to the server on port 443, and negotiates a TLS session. HTTPS is mandatory on GitHub Pages; it encrypts the transfer, prevents tampering in transit, and is a prerequisite for modern browser features. The browser then sends an HTTP `GET` request for `/index.html` (or `/`, which the server maps to `index.html`). The server locates the file on disk and returns it in an HTTP response:

```
HTTP/2 200 OK
content-type: text/html; charset=utf-8
content-length: 9843
cache-control: max-age=600
etag: "6a1f…"
```

While parsing the HTML the browser discovers the `<link rel="stylesheet">` and each `<img>` and issues further `GET` requests for `assets/css/style.css`, the SVG images and the Google Fonts stylesheet. With HTTP/2 these requests are multiplexed over the same connection. Because nothing is computed on the server, every response is a plain file read: this is what makes a static site fast, cheap to host and easy to cache at a CDN edge.

Conditional requests keep repeat visits light. The browser sends `If-None-Match` with the previous `ETag`; if the file is unchanged the server replies `304 Not Modified` with no body. Errors follow the same protocol — a mistyped URL returns `404 Not Found`.

### 4.2 MIME types and rendering

The `Content-Type` header carries a MIME type that tells the browser how to interpret the bytes; browsers do not rely on the file extension.

| Asset                | MIME type                | Browser behaviour                                   |
|----------------------|--------------------------|-----------------------------------------------------|
| `*.html`             | `text/html`              | Parsed by the HTML parser into the DOM and rendered. Served as `text/html` (not `application/xhtml+xml`) so the XHTML-syntax pages work in every browser; the syntax remains valid XML for tooling. |
| `*.css`              | `text/css`               | Parsed into the CSSOM and applied to the DOM. Browsers in standards mode refuse stylesheets served with the wrong type. |
| `*.json`             | `application/json`       | Displayed as a pretty-printed tree in modern browsers, or downloaded; consumed as data by any client that requests it. |
| `*.svg`              | `image/svg+xml`          | Rendered as vector graphics in `<img>` and as a favicon. |
| JSON-LD in `<head>`  | `application/ld+json` (on the `<script type>` attribute) | Ignored by the rendering engine, read by crawlers. |

GitHub Pages sets these types automatically from the extension. If `data.json` were ever served as `text/plain`, a client fetching it programmatically would still receive the bytes, but strict clients would refuse to parse it — which is why correct MIME configuration is part of "web standards" compliance rather than an optional nicety.

---

## 5. Content architecture and CMS evaluation

### 5.1 Static content management in this project

Content is managed by editing `data/data.json` and mirroring the change in the relevant HTML page, then committing to Git. Git provides versioning, rollback and a review step (pull requests) at no cost. The `data.json` file keeps the content model explicit, so the site could be migrated to a template-driven system by writing one template per entity type without re-authoring content.

### 5.2 CMS selection justification (≈430 words)

The portfolio's hand-written HTML5/CSS/JSON architecture and a content management system such as WordPress solve different problems, and the right choice depends on who edits the content, how often, and what the site has to do.

**Where the static architecture wins.** For a single-author portfolio updated a few times a semester, the manual approach is technically superior on almost every axis. It has zero server-side attack surface: there is no PHP runtime, database, plugin ecosystem or admin login to compromise, whereas unpatched WordPress installations remain one of the most common vectors for website breaches. Hosting is free on GitHub Pages or Netlify and the site is served from a CDN, giving load times a database-backed CMS cannot match without a caching layer. Every change is a Git commit, so the full history is auditable and reversible. The total dependency footprint is a browser, which means the site will still render unchanged in a decade. Finally, complete control over the markup is what made it possible to ship validated XHTML, semantic landmarks, JSON-LD and WCAG-level contrast without fighting a theme's defaults.

**Where it costs.** The static approach couples content to markup. Adding a project means editing `data.json` *and* `projects.html`, and any inconsistency between them is a manual error. Editors must know HTML, use Git and understand the directory conventions. There is no media library, no scheduled publishing, no search, no comments and no user accounts. None of these matter for one engineering student; all of them matter once the site is a shared publication.

**Conditions that would justify migrating to a CMS.** Migration becomes the correct engineering decision when at least one of the following holds:

1. **Non-technical editors.** If a department administrator, a student society or a client needs to publish without touching code, a CMS's WYSIWYG editor and role-based permissions are the entire value proposition.
2. **Update frequency above roughly weekly.** Once content changes faster than a developer can reasonably review commits, the overhead of the manual workflow exceeds the overhead of maintaining a CMS.
3. **Team size greater than two or three contributors.** Concurrent editing, drafts, approval workflows and audit trails are solved problems in a CMS and awkward to reconstruct in Git for non-developers.
4. **Dynamic features.** Search, comments, form submissions stored server-side, personalisation or an editorial calendar require server logic that a static site cannot provide.
5. **Content reuse across channels.** If the same records must feed a website, a mobile app and a newsletter, a headless CMS (for example Strapi or Contentful) exposing `data.json`-style records over an API is the natural evolution of the current model — the JSON schema designed here would map almost directly onto its content types.

Until one of those conditions is met, the static architecture is not a compromise imposed by the brief; it is the cheaper, faster and more secure design.

---

## 6. Deployment

The repository is hosted on GitHub and published with GitHub Pages from the `main` branch root. A `.nojekyll` file disables Jekyll processing so files are served exactly as committed. HTTPS is enforced in the repository's Pages settings. Deployment steps are documented in `README.md`.
