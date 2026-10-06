# Better Basista

A starting website for an independent transparency portal for the Municipality
of Basista, Pangasinan, built under the [BetterGov.ph](https://bettergov.ph/)
BetterLGU initiative.

This is a **starter**, not a finished site: it implements exactly the four
pages (plus global footer) defined in the project's content guide — Home,
About, Projects & Tools, Get Involved — using the Word-for-Word text verbatim
and lightly adapting the Summarize-tagged sections for the web. It
deliberately does **not** include a Statistics, Government, or Transparency
page yet, because no sourced civic data (population, land area, officials,
budget) was provided for Basista at build time, and the sources checked
during setup disagreed with each other (land area alone is reported
differently across the Province of Pangasinan's own current and archived
profile pages). Publishing inconsistent numbers undermines the whole point of
a transparency site — better to add that page once the data is verified, the
same way Better Mapandan's Statistics page cites one source per figure.

Architecturally this follows the same pattern used for Better Mapandan: a
small Python build script assembling shared partials, so the header and
footer each live in exactly one file. Visually, it's deliberately distinct —
different palette (drawn from this project's own seal: blue, gold, earth
brown), different type pairing (Space Grotesk + Lora, the inverse of
Mapandan's serif-display/sans-body), different motifs (a hill-silhouette
divider instead of a sunburst, circular "seal" medallions instead of square
icon boxes, no emergency hotline bar since no numbers were supplied).

## Structure

```
build.py                 Assembles src/pages/*.html + src/partials/*.html
                          into the final static pages below.

src/
  partials/
    base.html             <head> + <body> shell
    header.html            Site nav (single source of truth)
    footer.html             Site footer, incl. {{REPO_URL}}
  pages/
    index.html              Front matter (title/description) + body content
    about.html               only — no repeated header/footer in these files.
    projects.html
    volunteer.html

assets/
  style.css              Design tokens + styles. Zero inline styles anywhere
                          in the generated HTML.
  script.js                Mobile nav toggle — the only JS on the site.
  logo.png                 Municipal seal, source for the header/footer marks
                            and every generated favicon.
  favicon.ico, favicon-*.png

index.html, about.html, projects.html, volunteer.html   <- BUILD OUTPUT.
                                                             Edit src/, not these.
README.md
```

## Editing content

1. Edit `src/pages/*.html` for page copy, or `src/partials/` for nav/footer
   changes.
2. Run `python3 build.py` to regenerate the root `.html` files.
3. Commit both the `src/` change and the regenerated output — the root files
   are what actually gets served.

## What's deliberately left out, and why

- **No Statistics page.** The sources checked while setting this up
  (pangasinan.gov.ph's current profile vs. its own archived page) disagree on
  land area and show population figures from different census years. Rather
  than pick one silently — the exact mistake flagged and fixed on Better
  Mapandan's Statistics page — this starter leaves the numbers out and says
  so on the About page, with a plan to add a sourced, dated version later.
- **No emergency hotline bar.** No verified numbers for Basista's
  MDRRMO/BFP/PNP were supplied; publishing invented ones would be worse than
  publishing none.
- **No fake "coming soon" project cards.** The Projects page says plainly
  that nothing is published yet, rather than mocking up dashboards with no
  real data behind them.
- **No volunteer contact email.** None was provided; the Get Involved page
  points to the GitHub org and BetterGov.PH's own channels instead of
  guessing at an address.

## Before you deploy

1. Rename this repository to `betterbasista` and set it up on GitHub, per
   the [BetterLGU guide](https://directory.bettergov.ph/guide).
2. Update `REPO_URL` in `build.py` (`SITE_CONFIG`) to the real repo URL, then
   rebuild.
3. Register `betterbasista.org` and point it at your host.
4. Start gathering sourced municipal data (officials, budget, services) so
   the Projects and (eventually) Statistics pages have real content behind
   them.
5. **Visual check before shipping:** the icons and the hill-silhouette hero
   divider were hand-written SVG and could not be rendered/previewed in this
   build environment (no browser or SVG rasterizer available) — give the
   site a look in an actual browser before publishing, same as recommended
   for Better Mapandan.

## Deploying

Any static host works — root `.html` files + `assets/` is the whole
deployable site. `src/` and `build.py` are maintainer-only.

- **GitHub Pages**: Settings → Pages → deploy from `main`, root folder.
- **Netlify / Vercel**: no build command, `/` as the publish directory.

## Once live

Register this LGU's entry in the
[BetterLGU directory](https://directory.bettergov.ph/) as 🔵 Planned, then
update it to 🟢 Active once deployed, per that repository's
`CONTRIBUTING.md`.
