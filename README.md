# Flooring Sales Engine™ — Quote Funnels

Static site on Vercel (https://flooring-sales-engine.vercel.app). No build step: each page is one self-contained HTML file.

| Path | What it is |
|---|---|
| `index.html` | Demo hub linking both demos |
| `demo-1/index.html` | Book-a-measure funnel (config: `window.FSE_CONFIG`) |
| `demo-2/index.html` | Castle Carpets concept demo: landing page + 6-step quiz (config: `window.CC_CONFIG`) |
| `docs/ghl-build-guide.html` | GHL build guide (fields, pipeline, workflows, scripts). Not deployed |
| `api/lead.js` | Vercel function that forwards enquiries to GHL. Not deployed until a client is connected |

`.vercelignore` keeps `README.md`, `docs/` and `api/` off the live site. `robots.txt` and `noindex` keep the demos out of Google.

## Deploy
`npx vercel@latest deploy --prod`

## Preview
Open any `index.html` in a browser. Resize to phone width or use device mode in DevTools.

## Customise demo 1 for a client
Edit only the `window.FSE_CONFIG` block at the top of `demo-1/index.html`:

| Section | What to change |
|---|---|
| `company` | Name, logo text or logo URL, phone, WhatsApp, website, showroom address |
| `brand` | 5 colours |
| `offer` / `welcome` | Price banner, headline, hero image |
| `questions` | Titles, options, images (swap Unsplash URLs for GHL media URLs) |
| `booking` | Time slots, closed days, notice period, GHL calendar links |
| `ghl.webhookUrl` | The client's GHL inbound-webhook URL |
| `tracking.metaPixelId` | Only if the pixel isn't already in the GHL funnel settings |

## Put it in GHL
1. Create a blank funnel page and set its background to `#F2F4F9`.
2. Add a **Custom Code** element.
3. Paste everything between `<!-- FSE START -->` and `<!-- FSE END -->`.

## Data sent to GHL
The funnel sends a JSON POST to `ghl.webhookUrl` at three points (`stage` field):
`lead_captured`, `callback_requested`, `appointment_booked`.

Fields: `first_name, last_name, full_name, email, phone, postal_code, flooring_type, rooms, area_size, timeframe, next_step, appointment_type, appointment_date, appointment_time, company, page_url, submitted_at` + any `utm_*`, `fbclid`, `gclid` from the URL.

While `webhookUrl` is empty (demo mode), each payload is logged to the browser console instead.

## Meta events
`Lead` (details submitted), `Schedule` (appointment booked), `Contact` (callback), plus custom `FSE_Start` / `FSE_Step`.

## URL prefill
`?name=…&email=…&phone=…&postcode=…` prefills the contact form.
