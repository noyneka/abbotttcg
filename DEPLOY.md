# Deploying AbbottTCG.com — the CARD STOP site

This `/website` folder is a complete, **self-contained static site**: one `index.html`
(inline CSS, one tiny inline script for the year — no build step, no paid dependencies)
plus an `/assets` folder of images. You can host it for free and point **abbotttcg.com** at it.

**Files in this folder**

| File / folder | Purpose |
|---|---|
| `index.html` | The entire website (all sections are marked with `[HTML] …` comments so they're easy to find and edit). |
| `assets/` | Site images — logo brand sheet, machine render + transparent cutout, elemental frame, host one-pager. |
| `CNAME` | Custom-domain file for **GitHub Pages** (contains `abbotttcg.com`). |
| `.nojekyll` | Tells GitHub Pages to serve files as-is (skip Jekyll processing). |
| `DEPLOY.md` | This guide. |

> **Quick local preview:** double-click `index.html` to open it in any browser. Everything (including the `assets/` images) loads with no server.

> **⚠️ Before you publish — delete `assets/_scratch/`.** It holds throwaway build files (grid overlays, cutout test composites) that were used while prepping the images. They're not referenced by the site. (They couldn't be removed automatically from this environment — just delete the `_scratch` folder on your computer.)

---

## ✅ Asset checklist — what to finish before (or shortly after) launch

The site is wired up and looks complete with the current placeholder assets. To make it fully yours:

- [ ] **Drop in a real logo.** The header currently *crops the brand sheet* (`assets/card-stop-logo-system.png`) down to the horizontal lockup as a stand-in. Export an **isolated, transparent-background CARD STOP logo** (e.g. `assets/card-stop-logo.png`), then in `index.html` search for **`LOGO PLACEHOLDER`** and follow the two-line note there (set the new `src`, and switch `.logo-crop img` to `position:static; width:auto; height:44px`).
- [ ] **Set final prices.** Search `index.html` for **`PRICE PLACEHOLDER`** (two spots — booster packs and mystery singles) and replace the `$X.XX` copy with your real, posted prices.
- [ ] **Add real machine photos (optional but recommended).** The hero uses a transparent cutout (`assets/card-stop-machine-cutout.png`) and "The machine" section uses the studio render (`assets/card-stop-machine.png`). Swap in on-site photos of an installed unit when you have them — a real placement photo builds a lot of trust.
- [ ] **Confirm the machine spec copy.** "The machine" section states **VTM Slim Tower 2.0**: free-standing tower, **43″** touchscreen, **tap / card (cashless)**, **456-pack** capacity, ~2 ft × 2 ft footprint. Verify these match your final unit and edit if needed.
- [ ] **Social preview image.** `index.html` sets an Open Graph image to `assets/card-stop-machine.png`. Swap for a purpose-built 1200×630 share image if you want a cleaner link preview.
- [ ] **Favicon.** A simple inline SVG favicon is included as a placeholder. Replace with an exported `.png`/`.ico` if you have brand favicons.
- [ ] **Set up the mailbox.** Make sure **hello@abbotttcg.com** exists (or is an alias/forwarder) so the "Email us / Host" buttons reach you. The phone link uses **(828) 536-9660**.
- [ ] **Host one-pager (optional).** The Host section links `assets/one-pager-reference.png` as a "view the one-pager" preview. Replace with a finalized PDF/PNG if you'd rather hand out a polished flyer.

Everything above is optional to go live — the site works today — but the logo and final prices are the two things worth doing first.

---

## Recommended: GitHub Pages (free, custom domain, free HTTPS)

### A. Put the site on GitHub
1. Create a free account at **github.com**.
2. Create a new **public** repository — e.g. `abbotttcg-site`.
3. Upload the **entire contents of this `/website` folder** to the repo **root** — that means `index.html`, `CNAME`, `.nojekyll`, **and the whole `assets/` folder** (drag the folder in so the images come along). Use **Add file → Upload files**, then **Commit changes**. *(If `.nojekyll` is awkward to upload because it starts with a dot, use **Add file → Create new file**, name it `.nojekyll`, leave it empty, and commit.)*
4. Go to **Settings → Pages**. Under **Build and deployment**: Source = **Deploy from a branch**; Branch = **main**, folder = **/(root)**; click **Save**.
5. Still on **Settings → Pages**, confirm **Custom domain** shows `abbotttcg.com` (the included `CNAME` sets this automatically). Once it verifies, tick **Enforce HTTPS** (can take a few minutes to ~an hour to provision).

### B. Point the domain (DNS at your registrar)
An apex/root domain cannot use a CNAME, so use **A records** for the apex and a **CNAME** for `www`:

**Apex `abbotttcg.com` — add these four `A` records (GitHub Pages):**
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```
*(Optional IPv6 `AAAA` records: `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`)*

**`www.abbotttcg.com` — add one `CNAME` record → `YOURUSERNAME.github.io.`**

Then, back in **Settings → Pages**, add `www.abbotttcg.com` too; GitHub redirects between the apex and `www` automatically.

> **Verify the IPs first.** Confirm the current GitHub Pages IP addresses at **docs.github.com** (search *"Managing a custom domain for your GitHub Pages site"*) before relying on them — they rarely change but it's a 30-second check. DNS changes can take ~10 minutes to 48 hours to propagate; HTTPS provisioning follows propagation.

---

## Alternative 1: Netlify (free)
1. Sign up at **netlify.com** → **Add new site** → **Deploy manually**, then drag the **whole `/website` folder** onto the upload area (or connect the GitHub repo). The `assets/` images upload with it.
2. **Site configuration → Domain management → Add a custom domain** → `abbotttcg.com` (add `www.abbotttcg.com` too).
3. Easiest path: point your registrar's **nameservers** to Netlify (Netlify DNS) and it sets up apex + `www` + SSL for you. Otherwise keep your registrar's DNS and add: apex **ALIAS/ANAME** → `your-site.netlify.app`, and `www` **CNAME** → `your-site.netlify.app`.
4. Free HTTPS is automatic. **Delete the `CNAME` file** if you use Netlify — it's GitHub-specific.

## Alternative 2: Cloudflare Pages (free)
1. Sign up at **pages.cloudflare.com** → **Create a project** → upload the `/website` folder (or connect the repo).
2. Project → **Custom domains** → add `abbotttcg.com` and `www.abbotttcg.com`.
3. If the domain's DNS is on Cloudflare, records are added automatically (Cloudflare supports **CNAME flattening** at the apex). Free HTTPS is automatic. **Delete the `CNAME` file** if you use Cloudflare.

---

## Pointing Abbott.LLC at the same site
You also own **Abbott.LLC**. Recommended: make **AbbottTCG.com** the primary site and **redirect Abbott.LLC → AbbottTCG.com**.
- Most registrars offer free **domain forwarding / URL redirect** — set `abbott.llc` (and `www.abbott.llc`) to 301-forward to `https://abbotttcg.com`.
- Or add `abbott.llc` as a **second custom domain** on your chosen host so it serves the same site.

## Updating the site later
Edit `index.html` (or swap files in `assets/`) and re-upload — GitHub: commit the change; Netlify/Cloudflare: re-drag the folder or push to the connected repo. All three hosts redeploy automatically.

---

### Notes
- **GitHub Pages is the recommended** free option that supports a custom apex domain with free HTTPS; the included `CNAME` and `.nojekyll` target it. If you choose Netlify or Cloudflare Pages, just delete the GitHub-specific `CNAME` file.
- The site is fully static with **no backend**; the "Email us / Host" buttons open the visitor's email app (a `mailto:` link) and the phone button uses a `tel:` link. If you later want an in-page contact form, Netlify Forms or a free form service can be added without changing hosts.
- Keep it honest: the copy makes **no earnings claims** and states that Abbott Trading Card Group is **independent and not affiliated with any game publisher** — keep that language intact as you edit.
