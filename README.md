# Personal Portfolio — Future Computer Engineer

A multi-page portfolio built with **HTML5 (XHTML syntax), CSS3 and JSON only** — no JavaScript, no CMS, no site generator.

**Live site:** https://YOUR-USERNAME.github.io/portfolio/  ← update after deploying

## Features (all CSS, no JavaScript)
Light/dark theme switch · full-screen menu · organisations marquee · scroll-snap photo strip · tabbed skills · project pop-ups · accordions · back-to-top · printable CV · mailto contact form

## Features (all CSS-only)
Dark/light theme switch · full-screen menu overlay · tools marquee · hover-reveal portfolio · scroll-snap photo strip · `:target` project pop-ups · accordions · printable CV · back-to-top

## Pages
Home · About Me · Educational Background · Technical Skills · Projects · Hobbies & Interests · Curriculum Vitae · Contact Me

## Structure
```
index.html, about.html, education.html, skills.html, projects.html, hobbies.html, cv.html, contact.html
assets/css/style.css        single mobile-first stylesheet
assets/images/              SVG portrait, project thumbnails, favicon
data/data.json              content model for all site entities
docs/technical-report.md    design rationale, JSON schema, HTTP/MIME overview, CMS evaluation
```

## Before you publish
The content is already filled in from your CV. Check these:

| Placeholder | Where |
|---|---|
| Skill levels (1–4), module names, project descriptions | `skills.html`, `education.html`, `projects.html`, `data.json` — adjust to match you |
| `assets/images/portrait.svg` | replace with a portrait photo (portrait orientation, e.g. `portrait.jpg`) — it is cropped into an arch; update the `<img>` paths in `index.html` and `about.html` |
| `assets/images/photo-1.svg` … `photo-6.svg` | replace with six of your Damipictures photos (portrait orientation) and update `data.json` `gallery` + the `<img>` paths in `index.html` and `hobbies.html` |
| `assets/images/project-*.svg` | replace with real project images (16:9) |
| `YOUR-USERNAME` | GitHub links in footer/contact/projects, `README.md`, `docs/technical-report.md`, `base_url` in `data.json` |

Keep the XHTML rules when editing: lowercase tags, quoted attributes, and self-closed void elements (`<img … />`, `<br />`, `<input … />`).

## Deploy to GitHub Pages
1. Create a new **public** repository on GitHub named `portfolio` (or any name).
2. In this folder run:
   ```bash
   git init
   git add .
   git commit -m "Initial portfolio"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/portfolio.git
   git push -u origin main
   ```
3. On GitHub open **Settings → Pages**. Under *Build and deployment* choose **Deploy from a branch**, branch `main`, folder `/ (root)`. Save.
4. Wait about a minute, then visit `https://YOUR-USERNAME.github.io/portfolio/`. Tick **Enforce HTTPS** on the same settings page.
5. Paste the live URL into `README.md`, `docs/technical-report.md` and `data.json` (`site.base_url`), commit and push.

## Validate
- HTML: https://validator.w3.org/ (paste each page or the live URL)
- CSS: https://jigsaw.w3.org/css-validator/
- JSON: `python3 -m json.tool data/data.json`
- Structured data: https://validator.schema.org/

## Licence
© 2026 Olokoba David Oluwadamilola. All rights reserved.
