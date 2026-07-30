# Site

The live site. Five pages, static HTML, no build step.

```
site/
  index.html        Home. The offer, the honesty, the hosts, the FAQ.
  workshops.html    Booking hub. Calendar embed, agenda, how to join.
  learn.html        Articles and workbooks. Empty for now, honestly so.
  about.html        Story, hosts, how we get paid, four promises.
  contact.html      Phone first, what happens when you call, question form.
  free-class.html   Paid-traffic landing page for Meta ads. See below.
  thank-you.html    Post-booking page. Fires the conversion event.
  lp/               Four landing page directions kept for reference.
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
- The star rating is deliberately not stated anywhere. Every review seen so
  far is five stars, but the aggregate Google displays has not been confirmed,
  and an unverified rating is not worth the risk. Confirm it and it can go in.
- The write-a-review link (`g.page/r/.../review`) is still needed. It does NOT
  go on the site; it goes in post-workshop follow-up emails and texts. Get it
  from Google Business Profile under "Ask for reviews."

## The review count

The site states **68**, hardcoded in two places: the `.rev-num` figure and the
headline on index.html, and the closing line on workshops.html.

Specific beats rounded here. "68" reads as a number someone actually looked
up; "65+" reads as marketing hedging, and the voice rules call for specific
numbers. The count also only realistically moves up, so a stale figure
understates rather than overstates, and a visitor who clicks through to find
more than we claimed trusts us more, not less.

Refresh it quarterly, or any time it has drifted far enough to be worth the
edit. If it ever drops below what the page claims, fix it that day.
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

Reviews currently used, all complete and unedited:

| Review | Where | Why it was chosen |
|--------|-------|-------------------|
| Betty Hunt | home, workshops | Names the seminar and the outcome we promise: she understood Medicare when she left |
| Jeff Jones | home, workshops | Attended a live session, describes the presentation |
| Heyward Harrington | home | "without being pushy" is the No-Pitch Promise in a stranger's words |
| Doug Stikeleather | home | "explained everything so I could easily understand" |
| Mary Roy | home | "made me feel very comfortable" |
| Robert Estep | home | Warm and human, no plan references |

Deliberately not used, despite being strong: anything naming Medicare
Advantage, supplement, gap, or health plans (Teresa Ratliff, Brian Tune, Amy
Wellman, Michelle Wu, Mark Pearson), because plan-type references undercut
the educational-event framing. Also skipped: life-insurance reviews (Zach
Hopewell), reviews truncated by Google's "View full review" (Brenda Ball
Heilig, John Mello) since a partial quote cannot be verified as written, and
Gwen Perkins, whose review mentions food at an in-person seminar and would
mislead someone booking a webinar.

## Meta Pixel and conversion tracking

Pixel `27176602235306137` is installed on every page, so Meta can build
retargeting audiences from anyone who visits, not only ad clickers.

Events:

| Event | Where | Meaning |
|-------|-------|---------|
| `PageView` | every page | baseline traffic |
| `InitiateCheckout` | /free-class and /workshops | first interaction inside the booking widget |
| `Schedule` + `Lead` | /thank-you | a completed booking |

**The one setup step left:** in GoHighLevel, open the calendar settings and set
the post-booking redirect to `/thank-you`. Until that is done, completed
bookings are not counted, because the booking finishes inside a third-party
iframe that the pixel cannot see. `InitiateCheckout` will still fire, so
optimize toward that only until the redirect is live.

Optimize campaigns for `Schedule` once bookings are flowing.

## The ad landing page (/free-class)

The Evening direction, chosen from four. Deep green with gold, Source Serif 4
and Source Sans 3, the calendar embedded directly on the page so nothing gets
typed twice. No nav links, because a landing page should have no exits.

The earlier single-viewport version is in git history. The calendar embed
means mobile scrolls; removing a click and a re-typed form was the better
trade.

### A compliance correction worth remembering

The Evening draft carried the line "help you enroll only if you decide to,"
borrowed from the reference page we took inspiration from. That page sells
one-to-one consultations. Ours carries an educational-event disclaimer that
says no enrollment is taken. Both cannot be true on the same page, and on a
Medicare ad that is real exposure. The copy now says the class explains
options and answers questions, and nothing is sold.

If the GoHighLevel calendar behind this page ever books one-to-one
appointments rather than the class, the page needs different copy AND
different compliance language, including Scope of Appointment handling. Do not
mix the two framings.

**Everything sits above the fold on every realistic device.** That was the
hard constraint and it drove the layout. Type scales against viewport *height*
as well as width (`clamp(30px, min(6.1vw, 7.2vh), 70px)`), so a short screen
shrinks the headline rather than pushing the button out of view. Verified at
375x667, 360x800, 390x844, 430x932, 768x1024, 1366x768, 1920x1080.

Phones drop the review quotes and the three-step row. They are worth having on
desktop, but on a phone they push the CTA below the fold, which costs more than
they add. The proof that survives on mobile is the photo, the pull quote, and
the three stats.

Below 600px of height (landscape phones, very old devices) the page is allowed
to scroll. Clipping content a senior cannot reach would be worse than scrolling.

**Two-step opt-in.** The CTA opens a modal rather than putting a form on the
fold. The click is a micro-commitment that tends to lift completion, and it
frees the vertical space the headline and proof need.

**The handoff.** On submit the form redirects to the GoHighLevel booking widget
with `first_name`, `last_name`, `email`, and `phone` prefilled, and carries any
`utm_*` and `fbclid` parameters through so each market and campaign can be
measured. Verify the prefill parameter names against your GHL calendar setup.

**Before running traffic:**
- Paste the Meta Pixel base code into the marked slot in `<head>`. The page
  already fires `fbq('track','InitiateCheckout')` on CTA click and
  `fbq('track','Lead')` on submit if `fbq` exists.
- Optionally POST to a GHL webhook on submit as well, so a lead who abandons at
  the calendar step is still captured. Marked with a TODO in the script.
- The page is `noindex` on purpose so it does not compete with the main site.

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
