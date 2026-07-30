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

Drop these into `assets/images/` with these exact names. The pages already
reference them, so they appear as soon as the files exist. Until then, each
one falls back to a "Photo coming soon" panel, so nothing looks broken.

| File | What it is | Notes |
|------|-----------|-------|
| `austin-tyler.jpg` | Austin, alone | Portrait orientation. **In place.** Optimized to 1000×1250 / 128 KB. |

The site is host-cards-for-Austin-only for now. Andrew Horn is still credited
in the prose on both the home and about pages, he just has no card or photo
until a higher-resolution headshot exists. To restore the two-up layout later,
swap `.host--solo` back to the `.hosts` grid and add a second card.

Also parked until photos exist: `andrew-portrait.jpg`, and `austin-andrew.jpg`,
the candid of both hosts that opened the about page.

Optimize any new photo before committing: roughly 1000px on the long edge,
JPEG quality 82, progressive. A 1.6 MB original becomes about 128 KB with no
visible difference at the sizes the site renders.

Per the brand photography direction: real places (office, kitchen table,
porch), natural light, no gradient studio backdrops, no stock, no AI-generated
people.

## Placeholders still to replace before launch

- `License #[pending]` — license number, on about.html
- ZIP code for the Monarch Street office, if you want it shown
- The Google reviews block on index.html links to a Maps search as a working
  placeholder. Replace with the direct Business Profile review URL, and
  confirm the exact review count and star rating before either is stated.
- Verbatim review quotes, once screened (see below)
- Contact form on contact.html is a visual demo. Wire to GoHighLevel or an
  email endpoint before launch.

## Using the Google reviews

Do NOT add `AggregateRating` structured data for reviews collected on Google.
Google's own guidelines prohibit marking up third-party reviews as your own,
and self-serving review markup is a known penalty risk for local businesses.
Display them and link out instead, which is what the site does now.

Before quoting any review verbatim, screen it: no plan names, no benefit
details, nothing that reads as a plan endorsement, or it stops being an
educational-event page. Quote exactly as written, attribute as Google shows
it, and never edit a quote in a way that changes its meaning.

## Icons

`assets/icons/` holds the favicon set, generated from the Rocking Chair Check.
Per the brand book, 16px and 32px use the check alone on a Bluegrass tile
because the rocker muddies at that size; 180px and up carry the full mark.
`site.webmanifest` covers Android home-screen installs.

To regenerate after a logo change, redraw at 16x supersample and downscale.

## The booking calendar

The GoHighLevel iframe takes several seconds to render and shows nothing while
it does, which on the main conversion page reads as broken. `workshops.html`
now handles this:

- `preconnect` to link.runonforge.us, and `form_embed.js` loads deferred from
  `<head>` rather than at the end of the body
- A branded loading panel covers the frame until the widget is actually drawn.
  The iframe `load` event fires before the widget finishes rendering, so the
  reveal waits for either a postMessage from the GHL origin or load plus an
  800ms grace, whichever lands first
- After 12 seconds with no widget, the panel swaps to the phone number and
  email so a slow calendar never costs a booking

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
