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
- [x] macOS button (added 6 October 2026; pinned to v2.4.0 from 21 to 29 October 2026, until the 2.5.0 Mac build was attached): `https://github.com/Kai702/MedResearch-Suite-Website/releases/latest/download/MedResearch-Suite-arm64.dmg`
- [x] All three buttons resolve to the v2.5.0 files: 251 MB, 313 MB and the 264 MB disk image
- [x] The FAQ states the same sizes and the Mac requirements (Apple silicon, macOS 14 (Sonoma) or newer)
- [x] The 2.5.0 Mac build (signed by Meet Radhanpura, notarized and stapled) was attached to v2.5.0 on both repositories on 29 October 2026, the macOS button pointed back at `latest`, and the FAQ's 2.4.0 sentence removed
- [ ] The 2.5.0 Mac file is built from a tag that has the network changes of 15 October 2026. Widen "the
      Windows version" in the FAQ's offline answer to cover the Mac once the Mac build is checked behind a proxy (needs a Mac)

The file names carry no version number, so a new release needs no link change. But every `latest`
link asks for the file of that name on whichever release is marked **Latest**, so a release only
works as the latest if every file a `latest` link asks for is attached to it (the two Windows files
and the disk image). Publish the release, with its files attached, before
deploying a site that describes it; until then the buttons serve the previous release.

On 6 October 2026 a release carrying only the DMG (tag `2.5.0`) was published as the latest, and both
Windows buttons returned "not found" for about six hours, until v2.4.0 was marked latest again. To
add a file to an existing version, attach it to that version's release instead of making a new one.

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

- [ ] Attach every file a `latest` link asks for to the release that will be the latest: `MedResearch-Suite.exe`, `MedResearch-Suite.zip` and, once the macOS button is no longer pinned, `MedResearch-Suite-arm64.dmg`, under exactly those names
- [ ] `SHA256SUMS.txt` lists all three. `tools/release_files.py checksums <folder> [<existing SHA256SUMS.txt>]` hashes the disk image when it is in the folder and otherwise keeps its line from the release's current file, and `tools/publish_release.sh` passes that file, so rebuilding the Windows files keeps the Mac line. A brand new release has no current file, so its disk image must be in the folder or added by hand
- [ ] Update the download sizes in the FAQ ("Which operating systems are supported?": the installer, the portable build and the disk image)
- [ ] Run `tools/validate_stats.py`: it fails if the harness size on the page ("583 checks, 343 of them") or the headline count ("64 statistics") disagrees with the harness or the page's accuracy tables
- [ ] Put the same `website/index.html` in both repositories, byte for byte (compare `git hash-object`). Merging it in the app repository does not deploy it; pushing the website repository does
- [ ] Check the live site with a cache-busting query string (`?cb=...`), since Cloudflare caches pages at its edge
- [ ] **The signed update file** (from the first build that carries the updater): on the owner's PC, run
      `tools/sign_update.py sign <folder> --notes <notes.txt>` on the folder CI handed over (add `--existing` with the
      release's current `update.json` once it has one), then `tools/publish_release.sh` for both repositories. It
      refuses without a valid signed file once the release carries one, and puts `update.json.sig` and then
      `update.json` up last. Write the notes (What's New) in plain sentences; they are shown as text, at most 4,000
      characters.
- [ ] Before publishing, run `node tools/demo_hospital_networks.js --exe "<the built MedResearch Suite.exe>"` on the
      built app, as for every release

## 5a. The Day the First Build With the Updater Is Published (Once)

Copies installed before it have no updater, so this build is the last manual download for everyone. These changes go
out together with it, never before, so the site never describes a build that is not out yet:

- [x] The download section and the release notes say that this build updates itself, and that copies installed before
      it must be installed by hand once
- [x] "Does my patient data leave my computer?" and "Does it work offline?" mention the daily update check (sends
      nothing about the person or their data; Preferences turns it off; offline it says nothing)
- [x] The hospital IT answer (`#hospital-it`) and its JSON-LD copy name the update check at github.com, with downloads
      from release-assets.githubusercontent.com
- [x] The citation box cites the build date shown in About, not a version number: set `built` in the citation script
      of `website/index.html` and the box's default text to the published build's date (5 November 2026 for the build
      of run 42130496240, if that is the one published)
- [x] `CITATION.cff`: `version` and `date-released` to the published build's date (it still describes 2.5.0, the build
      published before the updater), its message to "cite the build date your copy shows under About", and the Zenodo
      DOI the website cites (10.5281/zenodo.23915317)
- [x] Mark the first published build here, with its build date and build number: built 5 November 2026, 23:39 UTC, build 42130496240 (the CI run ID, from commit 99ca82b), published on both v2.5.0 releases on 6 November 2026

---

## 6. Claims to Re-Check When the App Changes

These are printed on the page and stop being true if the app moves on.

- [ ] **64 statistics** cross-checked against R (`tools/validate_stats.py` enforces the count)
- [ ] **583 checks, 343 of them against R** (enforced the same way)
- [ ] **7 workspaces** (Discovery, Sample Size, Analytics, Meta-Analysis, Network Meta-Analysis, Diagnostics, Visualizations), in the same order as the app's tab bar
- [ ] **18 HIPAA identifiers** scanned by the Safe Harbor helper
- [ ] The HIPAA answer's identifier figures: about 186,000 first names and 800,000 surnames, 98% of 521 identifier columns from 68 name origins, Aadhaar, NHS and card numbers confirmed by check digits. Re-check if `backend/phi_data/`, `backend/phi_detect.py` or `tools/test_phi_detection.py` changes
- [ ] The HIPAA answer's account of typed text: nothing that looks like an identifier is sent without two confirmations, and searches and AI requests refuse one outright, a plain name with no title excepted
- [ ] **300 DPI** exports in PNG, JPG, TIFF and SVG
- [ ] The R package attributions in the Accuracy drawers still match `tools/validate_stats.py`
- [ ] "Windows 10 and 11", "macOS 14 or newer on Apple silicon", "notarized by Apple", and the three download sizes
- [ ] The hospital IT answer (`#hospital-it`, added 5 November 2026): every address the app contacts (`main.js`
      `LITERATURE_HOSTS` and `AI_HOSTS`, the Feedback service, GitHub's download host), port 443 only, the local ports
      (`chooseBackendPort`, the relay), the program names and install folder, and "not code signed yet". Re-check it when
      any of these change, and when the Mac version gains the relay (the answer describes Windows only)

---

## 7. Decided

- [x] No analytics. The page loads no third-party script, font or stylesheet, and nothing reports on visitors, which keeps it consistent with "patient data never leaves the device"
- [x] `apple-touch-icon.png`, `robots.txt`, `sitemap.xml` and a styled 404 page are live
- [x] The demo video in "See it in use" is a local still image (`assets/video_poster.jpg`) until the visitor presses play; only then does the page load YouTube's player, from youtube-nocookie.com. Without JavaScript the still links to the video on YouTube. The video (`xOUcqMOq19A`, about 13 minutes) was recorded on an earlier version, which its caption says; update the caption if the video is replaced
