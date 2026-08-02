# Pre-publish checklist — MedResearch Suite website

Everything here is a placeholder or an assumption that **must be resolved before the site goes live**.
Nothing on this list can be verified from inside the repo, which is why it is a checklist and not a fix.

---

## 1. BLOCKER — the download buttons go nowhere

Both primary buttons in the `#download` section are `href="#"`. They are the whole point of the page and
they currently do nothing.

| Location | Button | Needs |
|---|---|---|
| `index.html` → `#download` | **Download for Windows** (Installer) | Real URL to the `.exe` installer |
| `index.html` → `#download` | **Portable Version** (Runs From a Folder) | Real URL to the portable `.zip` |

The three hero buttons intentionally point at `#download` (they scroll down), so they are fine — but they
lead to the two dead buttons above, so fixing these fixes the whole funnel.

**Where will the builds live?** GitHub Releases is the usual answer for this kind of app, e.g.
`https://github.com/<user>/<repo>/releases/latest/download/MedResearchSuite-Setup.exe`.
Using the `/releases/latest/download/<asset-name>` form means the link never needs updating again.

- [ ] Windows installer URL wired in
- [ ] Portable zip URL wired in
- [ ] Both links actually download when clicked (test in a private window)
- [ ] Confirm the stated size is still right (page currently claims **roughly 250 MB**)
- [ ] macOS button still says "Coming Soon" and is not a dead link when the build lands

---

## 2. BLOCKER — the domain is an assumption

I used `https://medresearchsuite.com/` throughout, because that is the domain implied by the
`hello@medresearchsuite.com` address. **This was inferred, not confirmed.** If the real domain differs,
every absolute URL below is wrong and link previews will silently render blank.

In `index.html` `<head>`:

- [ ] `<link rel="canonical" href="https://medresearchsuite.com/">`
- [ ] `<meta property="og:url" content="https://medresearchsuite.com/">`
- [ ] `<meta property="og:image" content="https://medresearchsuite.com/assets/app_meta.png">`
- [ ] `<meta name="twitter:image" content="https://medresearchsuite.com/assets/app_meta.png">`
- [ ] JSON-LD `url` and `image` fields (in the `application/ld+json` block)

Rules that are easy to get wrong:
- `og:image` **must be an absolute, publicly reachable URL**. A relative path will not render a preview.
- The image must be reachable **without a login and without a redirect**.
- Scrapers cache aggressively. Get it right before sharing the link anywhere.

---

## 3. BLOCKER — the contact mailbox does not exist yet

The page links `hello@medresearchsuite.com` in two places (the Download CTA and the footer). Until that
mailbox exists, every enquiry **bounces silently** — the worst possible failure for a "get in touch" CTA.

- [ ] Register the domain (if not already owned)
- [ ] Create the `hello@` mailbox, or a free forwarding alias to an inbox you actually read
- [ ] Send a test email to it from an outside address and confirm it arrives

---

## 4. Verify after deploying (not before)

- [ ] Site is served over **HTTPS** (OG scrapers and browsers both punish plain HTTP)
- [ ] Paste the URL into **WhatsApp, Slack, LinkedIn, and iMessage** — confirm the forest-plot preview card renders
- [ ] Facebook Sharing Debugger + LinkedIn Post Inspector to force-refresh scraper caches
- [ ] `assets/app_meta.png` loads directly at its absolute URL in a private window
- [ ] Test on a real phone, not just a narrow desktop window
- [ ] Confirm the section nav, glider, and scroll-spy work on touch

---

## 5. Claims to re-check if the app changes

These numbers are printed on the page and will quietly become lies if the app moves on.

- [ ] **44 statistics** cross-checked against R (counted from the validation tables on the page)
- [ ] **6 workspaces** (Discovery, Sample Size, Analytics, Meta-Analysis, Diagnostics, Visualizations) — must stay in the same order as the app's tab bar
- [ ] **18 HIPAA identifiers** scanned by the Safe Harbor helper
- [ ] **300 DPI** exports, in PNG / JPG / TIFF / SVG
- [ ] "Windows 10 and 11", "roughly 250 MB", "macOS build coming soon"
- [ ] The R package attributions in the Accuracy drawers still match `tools/validate_stats.py`

---

## 6. Optional, but worth deciding

- [ ] **Analytics.** If you want any, use something privacy-respecting and cookieless (Plausible, Fathom).
      Anything that phones home about visitors sits badly next to "patient data never leaves the device".
- [ ] **`apple-touch-icon`** for iOS home-screen bookmarks
- [ ] **`robots.txt` + `sitemap.xml`** once the domain is settled
- [ ] A real **404 page** in the site's styling
