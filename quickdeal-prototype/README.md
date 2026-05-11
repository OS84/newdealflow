# QuickDeal — Stakeholder Demo

A clickable prototype of the proposed deal-creation flow for Tower, deployable in under a minute.

## What this is

A self-contained HTML page (no backend, no install) that demonstrates the new deal-creation wizard. Synthetic data shaped after Tower's real distributions — 16 customers, 13 sales channels, 5 locations, 35 vehicle offerings. Submitting a deal renders the lifecycle in-place: pending manager approval → contract → payment → active.

Nothing leaves the page. The "Submit" action shows the JSON payload that would be written to the Stock record.

## Deploy in under a minute

**Recommended: Netlify Drop** (no signup required to start).

1. Go to [https://app.netlify.com/drop](https://app.netlify.com/drop) in your browser.
2. Drag this entire `quickdeal-prototype` folder onto the drop zone.
3. Wait ~10 seconds. You'll get a live URL like `https://snazzy-name-12345.netlify.app/`.
4. Share that URL with stakeholders.
5. (Optional) Click "Claim site" to sign in and rename it to something like `tower-quickdeal`.

The site stays live until you delete it. Each new deploy gives you a new URL unless you've claimed and connected the site.

## Alternatives

| Option | Setup | Best for |
|--------|-------|----------|
| **Vercel** | Run `npx vercel` in this folder, follow prompts | Teams already on Vercel |
| **GitHub Pages** | Push to a public repo, enable Pages in repo settings | If your team uses GitHub |
| **Cloudflare Pages** | Drag-and-drop at pages.cloudflare.com | Privacy-conscious teams |
| **Surge.sh** | `npm install -g surge && surge` in this folder | Fastest CLI option |
| **Internal hosting** | Drop `index.html` on Tower's S3/SharePoint/intranet | Keeps it inside your network |

All of these work because the prototype is a single HTML file with all dependencies served from public CDNs (React, htm, Inter font, JetBrains Mono).

## What to share with stakeholders

When you send the link, attach this short note:

> Hi — sharing a clickable prototype of the proposed new deal-creation flow.
>
> What it is: synthetic data, no backend — but every screen, state, and interaction is real.
>
> Try this path: pick **Driver A17** (an existing customer with an active package rental), choose **LIC — Dealership**, choose **Lease → LTOWPC**, and walk through to submit. After submitting you can drive the deal lifecycle (Approve as manager → Send contract → Take payment) using the demo controls.
>
> See `WALKTHROUGH.md` in the same folder for a full talk-track if you'd rather present it live.

## Files

- `index.html` — the prototype itself, fully self-contained
- `README.md` — this file
- `WALKTHROUGH.md` — a 20-minute stakeholder walkthrough script with three demo paths, anticipated questions, and decisions to surface
