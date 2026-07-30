# Site

The live site. Five pages, static HTML, no build step.

```
site/
  index.html        Home. The offer, the honesty, the hosts, the FAQ.
  workshops.html    Booking hub. Calendar embed, agenda, how to join.
  learn.html        Articles and workbooks. Empty for now, honestly so.
  about.html        Story, hosts, how we get paid, four promises.
  contact.html      Phone first, what happens when you call, question form.
  assets/
    site.css        Canonical stylesheet (see note below)
    images/         Photos go here
```

## Why each page inlines its CSS

Every page carries a copy of `assets/site.css` in a `<style>` block so it stays
a single portable file. That matters because these pages need to paste cleanly
into GoHighLevel, Netlify, or anywhere else with no build step and no broken
asset paths.

`assets/site.css` is the source of truth. Edit it first, then sync the change
into each page's style block. Tokens mirror `brand/tokens.css`.

## Photos needed

Drop these into `assets/images/` with these exact names and they slot in
automatically, replacing the labeled placeholders:

| File | What it is | Notes |
|------|-----------|-------|
| `austin-portrait.jpg` | Austin, alone | Natural window light, real setting, 1200×1500 or larger |
| `andrew-portrait.jpg` | Andrew, alone | Same distance and angle as Austin's so they pair |
| `austin-andrew.jpg` | Both together, candid | Landscape, 16:9 friendly. Mid-conversation beats posed |

Per the brand photography direction: real places (office, kitchen table,
porch), natural light, no gradient studio backdrops, no stock, no AI-generated
people.

Once the files are in place, replace the `<div class="placeholder">` blocks
with `<img src="assets/images/austin-portrait.jpg" alt="Austin Tyler">`.

## Placeholders still to replace before launch

- `(859) 555-0123` — real phone number, in every page header, footer, and body
- `hello@joinmedicareworkshop.com` — real email
- `Andrew [Last Name]` — Andrew's full name
- `License #[pending]` — both license numbers, on about.html
- `[Street address to be added]` — office address, on contact.html
- Testimonials on index.html — real, permissioned quotes
- Contact form on contact.html is a visual demo. Wire to GoHighLevel or an
  email endpoint before launch.

## Deployment (Vercel)

`vercel.json` at the repo root tells Vercel to serve this folder as the web
root:

```json
{ "outputDirectory": "site", "cleanUrls": true, "trailingSlash": false }
```

`cleanUrls` means pages are served without the `.html` extension, so internal
links use `/workshops`, `/learn`, `/about`, `/contact`, and `/` for home.
`404.html` is served automatically for unknown paths.

Two things to know:

1. **Vercel deploys production from the default branch (`main`).** Work on a
   feature branch gets a preview URL, not the live domain. Merge to `main` to
   go live.
2. Because of `cleanUrls`, opening these files directly from disk
   (`file://`) will not navigate between pages. To preview locally, serve the
   folder: `cd site && python3 -m http.server 8000`, then visit
   `localhost:8000`.

## Notes

- The booking calendar on workshops.html is the GoHighLevel embed. One calendar
  serves all markets; track market with UTM parameters on the page URL rather
  than duplicating calendars.
- The `allow="payment"` attribute from the default GHL snippet was kept as-is.
  Consider removing it, since nothing here is ever paid.
- Fonts load from Google Fonts. The fallback stack (Iowan Old Style / Palatino,
  Gill Sans / Trebuchet) keeps the same voice if they fail.
