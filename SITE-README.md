# PROMAXX Engineering website

Static website built from the Claude Design handoff in `promaxx-website/` (Home + Contact Us).
No build step and no framework: plain HTML, CSS and JavaScript. Works on any static host
(Hostinger, Netlify, Cloudflare Pages, GitHub Pages, cPanel).

## What is where

| Path | What it is |
|---|---|
| `site/index.html` | Home: hero, intro, services, workforce, why PROMAXX, approach, footer |
| `site/contact.html` | Contact Us: hero, call/email cards, enquiry form, footer |
| `site/solar-semiconductor.html` | Solar & Semiconductor (high-purity installation): capabilities, systems, industries, engagement model, quality standards, process engineering, CTA. Content from the client's PDF, illustrations cropped from it (`tools/source/solar/`). Teaser card on Home links here. |
| `site/orbital-welding.html` | Orbital Welding Services: why orbital, what we weld, gas services, 6-step process, documentation and standards, industries. Original wording; scope modelled on semi-gensys.com's orbital page, claims kept to what the client's PDF supports. |
| `site/css/style.css` | All styles (design tokens at the top) |
| `site/js/main.js` | Nav, scroll reveals, hero parallax, ambient image motion, form |
| `site/img/` | WebP images (20 MB of PNGs reduced to 1.25 MB) |
| `site/favicon*`, `icon-*`, `apple-touch-icon.png`, `og-image.jpg` | Icons and link-preview image |
| `site/robots.txt`, `site/sitemap.xml`, `site/site.webmanifest` | SEO and app metadata (domain set to promaxxeng.com) |
| `tools/optimize_images.py` | Rebuilds `site/img` and the icons from `promaxx-website/project/assets` |

## Preview locally

```bash
python -m http.server 5173 --directory site
```

Then open http://localhost:5173

## Go live on promaxxeng.com: GitHub + Netlify + Hostinger DNS

State checked 20 Nov 2026: promaxxeng.com is still served by Framer, the DNS records are in
Hostinger (nameservers `ns1/ns2.dns-parking.com`), and email runs on Proton Mail. The code is in
GitHub at `metricascentai-lang/promaxx-website` (branch `main`). `netlify.toml` tells Netlify to
publish the `site/` folder with no build step. Netlify values were re-checked in its docs the same day.

**Step 1: Move the code to the client's GitHub** (skip it to keep the repo on your account)
1. The client creates a free GitHub account and sends the username.
2. On github.com/metricascentai-lang/promaxx-website go to **Settings → General → Danger Zone →
   Transfer**, then enter the client's username.
3. The client accepts the transfer in GitHub's email **within 24 hours**. You stay on as a collaborator.

**Step 2: Create the Netlify site from GitHub** (in the Netlify account that will own the site)
1. Sign up at https://app.netlify.com using **Sign up with GitHub**.
2. **Add new project → Import an existing project → GitHub**, then authorise Netlify for the repository.
3. Pick `promaxx-website`, branch `main`. Leave the build command empty. Publish directory `site`
   is filled in by `netlify.toml`.
4. Click **Deploy**. You get an address like `something-123.netlify.app`. Open it and check the
   pages: Home, Services dropdown, Solar & Semiconductor, Orbital Welding, Contact.
5. Optional: **Project configuration → Change project name**, e.g. `promaxx-engineering`.

**Step 3: Add the domain in Netlify**
**Domain management → Add a domain** → `promaxxeng.com` → **Verify** → **Add domain**. If asked,
choose external DNS (keep DNS at Hostinger), **not** Netlify DNS. Netlify adds `www` too.

**Step 4: Point the domain in Hostinger**
hPanel → **Domains → promaxxeng.com → DNS / Nameservers → DNS records**:

| Action | Type | Name | Current value (Framer) | New value |
|---|---|---|---|---|
| Delete | A | `@` | `31.43.160.6` | — |
| Delete | A | `@` | `31.43.161.6` | — |
| Add | A | `@` | — | `75.2.60.5` (TTL 3600) |
| Edit | CNAME | `www` | `sites.framer.app` | `your-project-name.netlify.app` |

**Do not touch** the MX records (`mail.protonmail.ch`, `mailsec.protonmail.ch`) or the TXT records
(`protonmail-verification=…`, `v=spf1 include:_spf.protonmail.ch ~all`). They run @promaxxeng.com email.
If Hostinger offers an ALIAS type, `@` ALIAS → `apex-loadbalancer.netlify.com` is Netlify's first
choice. The A record works fine too.

**Step 5: HTTPS**
Netlify → **Domain management → HTTPS → Verify DNS configuration**. The free certificate issues
automatically, usually within minutes. The DNS change reaches everyone within a few hours.

**Step 6: Check and tidy up**
1. Open https://promaxxeng.com and https://www.promaxxeng.com: both should show the new site with
   the padlock. Send a test email to info@promaxxeng.com to confirm mail still arrives.
2. In Framer, remove the custom domain from the project and switch off auto-renew (paid to 7 Mar 2027).
3. Switch off the GitHub Pages preview (repo **Settings → Pages → Unpublish**), so the only live
   copy is promaxxeng.com.

**Future updates**
Every push to `main` on GitHub makes Netlify republish the site automatically, usually within a
minute. No more manual uploads.

The `deploy/promaxx-site/` folder and zip are kept only as a fallback for a manual upload
(Netlify Drop, or another host's `public_html`).

## Other hosts

Upload the contents of `deploy/promaxx-site/` to the web root (for example `public_html`).

## Contact form: needs one setting before launch

Until a form backend is set up, **Submit** opens the visitor's email app with the message
already addressed to info@promaxxeng.com. Nothing is lost, but the visitor still has to press send.

To have the form send by itself:

1. Create a free form key at https://web3forms.com, using info@promaxxeng.com.
2. In `site/contact.html`, on the `<form id="contact-form">` tag, set
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
