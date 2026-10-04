# Website Checklist: MedResearch Suite

What the site at https://medresearchsuite.com depends on, and what to re-check when the app or a
release changes. Every item marked done was checked against the live site on 3 October 2026.

The site is served by Cloudflare Pages from `website/` in `Kai702/MedResearch-Suite-Website`; a push
to that repository's `main` is the deploy. This file sits outside `website/`, so it is not published
(the live URL returns 404). Keep it identical in both repositories.

---

## 1. Downloads (Done)

- [x] Installer button: `https://github.com/Kai702/MedResearch-Suite-Website/releases/latest/download/MedResearch-Suite.exe`
- [x] Portable button: `https://github.com/Kai702/MedResearch-Suite-Website/releases/latest/download/MedResearch-Suite.zip`
- [x] Both resolve to the current v2.4.0 files (250 MB and 312 MB)
- [x] The FAQ states the same sizes: "a 250 MB installer or a 312 MB portable build"
- [x] The macOS row reads "coming soon" and is not a link

The file names carry no version number, so a new release needs no link change. Publish the release,
with its files attached, before deploying a site that describes it; until then the buttons serve the
previous release.

---

## 2. Domain and Link Previews (Two Manual Checks Left)

- [x] medresearchsuite.com is live, and plain HTTP redirects to HTTPS
- [x] `canonical`, `og:url`, `og:image`, `twitter:image` and the JSON-LD `url` and `image` all use `https://medresearchsuite.com/`
- [x] `assets/app_meta.png` loads at its absolute URL with no redirect
- [ ] Paste the URL into WhatsApp, Slack, LinkedIn and iMessage and confirm the preview card renders
- [ ] After any change to the preview image, refresh the scrapers' caches with the Facebook Sharing Debugger and the LinkedIn Post Inspector

---

## 3. Contact Mailbox (Needs the Owner)

The page links `support@medresearchsuite.com` in two places. Mail for the domain is routed to
Porkbun's forwarding servers (`fwd1.porkbun.com`, `fwd2.porkbun.com`), so a forward can exist, but
whether `support@` reaches an inbox that is read cannot be checked from here.

- [ ] Confirm a `support@` forward, or a catch-all, exists in Porkbun's email forwarding settings
- [ ] Send a test email to it from an outside address and confirm it arrives

---

## 4. Checks Only a Person Can Do

- [ ] Test the site on a real phone, not only a narrow desktop window
- [ ] Confirm the section nav, its glider and the scroll-spy work on touch

---

## 5. On Every Release

- [ ] Update the download sizes in the FAQ ("Windows 10 and 11 today, as a ... MB installer or a ... MB portable build")
- [ ] Run `tools/validate_stats.py`: it fails if the harness size on the page ("583 checks, 343 of them") or the headline count ("64 statistics") disagrees with the harness or the page's accuracy tables
- [ ] Put the same `website/index.html` in both repositories, byte for byte (compare `git hash-object`). Merging it in the app repository does not deploy it; pushing the website repository does
- [ ] Check the live site with a cache-busting query string (`?cb=...`), since Cloudflare caches pages at its edge

---

## 6. Claims to Re-Check When the App Changes

These are printed on the page and stop being true if the app moves on.

- [ ] **64 statistics** cross-checked against R (`tools/validate_stats.py` enforces the count)
- [ ] **583 checks, 343 of them against R** (enforced the same way)
- [ ] **6 workspaces** (Discovery, Sample Size, Analytics, Meta-Analysis, Diagnostics, Visualizations), in the same order as the app's tab bar
- [ ] **18 HIPAA identifiers** scanned by the Safe Harbor helper
- [ ] The HIPAA answer's identifier figures: about 186,000 first names and 800,000 surnames, 98% of 521 identifier columns from 68 name origins, Aadhaar, NHS and card numbers confirmed by check digits. Re-check if `backend/phi_data/`, `backend/phi_detect.py` or `tools/test_phi_detection.py` changes
- [ ] The HIPAA answer's account of typed text: nothing that looks like an identifier is sent without two confirmations, and searches and AI requests refuse one outright, a plain name with no title excepted
- [ ] **300 DPI** exports in PNG, JPG, TIFF and SVG
- [ ] The R package attributions in the Accuracy drawers still match `tools/validate_stats.py`
- [ ] "Windows 10 and 11", the download sizes, and "macOS coming soon"

---

## 7. Decided

- [x] No analytics. The page loads no third-party script, font or stylesheet, and nothing reports on visitors, which keeps it consistent with "patient data never leaves the device"
- [x] `apple-touch-icon.png`, `robots.txt`, `sitemap.xml` and a styled 404 page are live
