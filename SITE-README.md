# PROMAXX Engineering website

Static website built from the Claude Design handoff in `promaxx-website/` (Home + Contact Us).
No build step and no framework: plain HTML, CSS and JavaScript, in the `docs/` folder.
Hosted on GitHub Pages straight from `docs/` on branch `main` (see "Hosting" below).

## What is where

| Path | What it is |
|---|---|
| `docs/index.html` | Home: hero, intro, services, workforce, why PROMAXX, approach, footer |
| `docs/contact.html` | Contact Us: hero, call/email cards, enquiry form, footer |
| `docs/solar-semiconductor.html` | Solar & Semiconductor (high-purity installation): capabilities, systems, industries, engagement model, quality standards, process engineering, CTA. Content from the client's PDF, illustrations cropped from it (`tools/source/solar/`). Teaser card on Home links here. |
| `docs/orbital-welding.html` | Orbital Welding Services: why orbital, what we weld, gas services, 6-step process, documentation and standards, industries. Original wording; scope modelled on semi-gensys.com's orbital page, claims kept to what the client's PDF supports. |
| `docs/css/style.css` | All styles (design tokens at the top) |
| `docs/js/main.js` | Nav, scroll reveals, hero parallax, ambient image motion, form |
| `docs/img/` | WebP images (20 MB of PNGs reduced to 1.25 MB) |
| `docs/favicon*`, `icon-*`, `apple-touch-icon.png`, `og-image.jpg` | Icons and link-preview image |
| `docs/robots.txt`, `docs/sitemap.xml`, `docs/site.webmanifest` | SEO and app metadata (domain set to promaxxeng.com) |
| `tools/optimize_images.py` | Rebuilds `docs/img` and the icons from `promaxx-website/project/assets` |

## Preview locally

Links use clean paths (`/contact`, `/orbital-welding`), so use a server that maps them to `.html`:

```bash
npx serve docs
```

(`python -m http.server` serves the pages but cannot follow the clean links.)

## Hosting: GitHub Pages from `docs/` (live on promaxxeng.com since 25 Nov 2026)

Status 25 Nov 2026: Hostinger DNS switched by the domain owner, all 8 Proton email records verified
unchanged against `handover/email-dns-before.txt`, Let's Encrypt certificate issued for
promaxxeng.com and www (auto-renewed by GitHub). The domain sits in a Hostinger account that only
the domain owner can open.

- Code: github.com/PromaxxEng/promaxx-website (owner PromaxxEng; metricascentai-lang is a collaborator with push access).
- GitHub Pages publishes the `docs/` folder of branch `main`. **Every push to `main` goes live
  automatically** within about a minute. `docs/.nojekyll` turns off GitHub's Jekyll processing.
- Why not Netlify / Vercel / Cloudflare: Netlify Free (projects created after 19 Aug 2026) forces a
  "Powered by Netlify" badge; removing it needs Personal ($9/month). Vercel's free Hobby plan is
  non-commercial only. Cloudflare Pages needs the domain's nameservers moved to Cloudflare, which
  puts the Proton email records at risk. GitHub Pages: free, no badge, only website records change.
- GitHub's rule: Pages may not be used to run an online business or a site "primarily directed at
  facilitating commercial transactions". This is an informational company site with no sales or
  payments, which in our reading is allowed.

### One-time setup (done by the PromaxxEng account; collaborators cannot change Pages settings)
1. Repo **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main`,
   folder `/docs` → Save**.
2. Same page, **Custom domain: `promaxxeng.com` → Save**. (Add the domain here **before** changing DNS.)
3. After DNS works, tick **Enforce HTTPS** (can take up to 24 hours to become available).

### DNS at Hostinger: website records only
hPanel → Domains → promaxxeng.com → DNS / Nameservers → DNS records. Nameservers stay
`ns1/ns2.dns-parking.com`.

| Action | Type | Name | Old value (Framer) | New value |
|---|---|---|---|---|
| Delete | A | `@` | `31.43.160.6` | — |
| Delete | A | `@` | `31.43.161.6` | — |
| Add | A | `@` | — | `185.199.108.153` |
| Add | A | `@` | — | `185.199.109.153` |
| Add | A | `@` | — | `185.199.110.153` |
| Add | A | `@` | — | `185.199.111.153` |
| Edit | CNAME | `www` | `sites.framer.app` | `promaxxeng.github.io` |

**Never touch these email records (Proton Mail), checked 24 Nov 2026:**
MX `@` → `mail.protonmail.ch` (10) and `mailsec.protonmail.ch` (20);
TXT `@` → `v=spf1 include:_spf.protonmail.ch ~all`; TXT `@` → `protonmail-verification=…`;
TXT `_dmarc` → `v=DMARC1; p=quarantine`;
CNAME `protonmail._domainkey`, `protonmail2._domainkey`, `protonmail3._domainkey` → `…domains.proton.ch`.

### After launch
- Framer: remove the custom domain and switch off auto-renew before 7 Mar 2027.
- Netlify: delete the `promaxx-website` project (no longer used).
- The old `/contact` link from the Framer site works on GitHub Pages without a redirect
  (GitHub serves `contact.html` for `/contact`).

## Contact form: needs one setting before launch

Until a form backend is set up, **Submit** opens the visitor's email app with the message
already addressed to info@promaxxeng.com. Nothing is lost, but the visitor still has to press send.

To have the form send by itself:

1. Create a free form key at https://web3forms.com, using info@promaxxeng.com.
2. In `docs/contact.html`, on the `<form id="contact-form">` tag, set
   `data-endpoint="https://api.web3forms.com/submit"`.
3. Paste the key into `<input type="hidden" name="access_key" value="">`.

Formspree also works: set `data-endpoint` to your `https://formspree.io/f/...` URL and leave `access_key` empty.
If sending fails, the form shows an error with the email address and phone number, so the visitor can still reach you.

## Changes from the design file

- **Contact form:** Email and Phone now sit in two equal columns. The design's grid left an
  empty third column on wide screens.
- **Phones:** the "Why PROMAXX" text no longer runs into the slanted divider. The
  "How We Work" pictures now span the full card instead of showing as a small thumbnail.
  The footer picture is darkened so the address stays readable.
- **Footer:** added a copyright line.
- **Hero video:** `hero.mp4` was in the bundle, but the final design uses the still tank-farm
  photo, so the video is not used.
