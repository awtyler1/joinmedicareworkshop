# The Medicare Workshop

Free Medicare education for Kentucky seniors, taught by licensed local agents.
Tyler Insurance Group, Lexington, Kentucky.

The live site plus the brand and editorial systems that keep every page and
every article consistent.

## Structure

```
site/          The live website. Five pages, static HTML, no build step.
brand/         The design and writing system. Read before building anything.
designs/       Earlier design directions, kept for reference.
content/       The published log that tracks article form and range.
vercel.json    Tells Vercel to serve site/ as the web root.
```

## Deployment

Vercel deploys `main` to production. `vercel.json` sets `site/` as the web
root with clean URLs, so pages live at `/`, `/workshops`, `/learn`, `/about`,
and `/contact`. Unknown paths get `site/404.html`.

To preview locally: `cd site && python3 -m http.server 8000`

## The system, in reading order

| Document | What it decides |
|----------|-----------------|
| `brand/brand-guidelines.html` | Logo, color, type, voice, components, photography, compliance |
| `brand/tokens.css` | The single source of truth for every design value |
| `brand/writing-style.html` | The Kitchen Table Voice, built from a study of the great explainers |
| `brand/article-forms.html` | Ten article forms, so range stays wide while the voice stays constant |
| `brand/editorial-checklist.md` | The gate every published piece passes: True, Useful, Clear |
| `site/README.md` | Page-by-page notes, photo specs, and remaining placeholders |

The rule that holds it together: if a new need arises, add it to the system
first, then use it. The system grows. Pages never freelance.

## Before launch

See `site/README.md` for the full list. The short version: real phone number,
real email, Andrew's last name, both license numbers, office address, real
testimonials, headshots, and the contact form wired to a real endpoint.
