# Baseera website (`marketing/site/`)

This is a static, bilingual (Arabic / English) site for the App Store **Marketing**, **Support** and **Privacy Policy** URLs. It has no build step, no framework, no cookies, no analytics, no trackers and no third-party requests.

| File | Purpose | App Store Connect field |
|---|---|---|
| `index.html` | Marketing page (hero, pillars, how it works, Tree of Light, privacy) | Marketing URL: `https://hassma0k.github.io/baseera-site/` |
| `privacy.html` | Privacy policy (no data collected) | Privacy Policy URL: `https://hassma0k.github.io/baseera-site/privacy.html` |
| `support.html` | FAQ and contact | Support URL: `https://hassma0k.github.io/baseera-site/support.html` |
| `terms.html` | Short Terms of Use, which point to Apple's Standard EULA | (linked from the App Store description) |
| `assets/style.css` | App palette (day, plus night via `prefers-color-scheme`) and typography | |
| `assets/lang.js` | Arabic/English switch. Keeps the choice only in the URL (`?lang=en`); no storage | |
| `assets/icon-day.svg` | App icon (Doorway of Light), copied from `brand/v2-option-C-doorway-of-light/icon-day.svg` | |
| `assets/fonts/` | Self-hosted subsets of Amiri, Noto Naskh Arabic and EB Garamond (WOFF2) with their OFL licences | |

## Before publishing: 2 replacements

1. **Support e-mail.** Replace every `alhassan.mu@gmail.com` placeholder in `privacy.html`, `support.html` and `terms.html`, including the `mailto:` links:
   ```bash
   grep -rl "\[SUPPORT_EMAIL\]" . | xargs sed -i 's/\[SUPPORT_EMAIL\]/you@example.com/g'
   ```
   Use the same inbox as the in-app report e-mail (`Config.feedbackEmail` in `ios/Baseera/App/Config.swift`).
2. **App Store link (after approval).** In `index.html`, replace both `href="#"` on the `.cta` buttons with the App Store URL (`https://apps.apple.com/app/id<APPLE_ID>`). Change the button text from "Coming soon" / «قريبًا» to "Download on the App Store" / «حمّل من App Store», or swap in Apple's official badge artwork from Apple's marketing resources (use it unmodified, per Apple's badge guidelines).

## Publish on GitHub Pages

1. On GitHub (account `hassma0k`), create a **public** repository named **`baseera-site`**.
2. Copy the **contents** of `marketing/site/` (not the folder itself) into the repository root, so `index.html` sits at the top level. You can leave this `README.md` in.
3. Commit and push to `main`.
4. Go to **Settings → Pages → Build and deployment**, set Source to *Deploy from a branch*, Branch to `main`, folder `/ (root)`, and click Save.
5. After a minute or two the site is live at `https://hassma0k.github.io/baseera-site/`. Check that all four pages load over HTTPS, then paste the URLs into App Store Connect (`marketing/APP-STORE-SUBMISSION.md`).
6. Optional: add an empty `.nojekyll` file at the root to skip GitHub's Jekyll processing.

Command-line version:
```bash
git clone https://github.com/hassma0k/baseera-site.git
cp -r "marketing/site/." baseera-site/
cd baseera-site && git add -A && git commit -m "Baseera website" && git push
```

## Design notes

- The palette and type match the app: bg `#F3EDE2`, surface `#FBF7EF`, ink `#23303A`, gold `#A8874F`; Amiri for headings, Noto Naskh Arabic for Arabic text, EB Garamond for English. Night colours apply automatically when the visitor's device is in dark mode.
- **Language:** Arabic is the default. English is chosen when the browser prefers English or the URL has `?lang=en`, and the toggle switches in place. `<html lang/dir>` is updated, so RTL/LTR layout, fonts and screen readers follow. Without JavaScript both languages are shown, Arabic first.
- Mobile-first. The layout is a single column up to 680 px with 16 px side gutters, and the pillars become three columns on wider screens.
- **Fonts are self-hosted, not loaded from Google Fonts.** Loading Google Fonts sends every visitor's IP address to Google. German courts have found this breaches the GDPR when done without consent (LG München I, 3 O 17493/20, January 2022), and the owner is based in Germany. It would also contradict the "no third parties" line in `privacy.html`. The subsets (Arabic, Latin and punctuation; about 600 KB in total) were made from the OFL fonts in `assets/fonts/` with `pyftsubset`. None of these fonts has a Reserved Font Name, so subsetting is allowed. If you prefer Google Fonts anyway, replace the `@font-face` block in `style.css` with the Google Fonts `<link>`, and add a sentence to the privacy policy saying that fonts are loaded from Google.
- No Quran text appears on the site, which avoids any risk of partial or unverified Ayah text outside the app.

## Legal note for a Germany-based owner

German law (§ 5 DDG, formerly § 5 TMG) may require an **Impressum** (legal notice with name, postal address and e-mail) on websites run on a business-like basis, which can include a free app's website. If that applies, add an `impressum.html` with the owner's real details and link it in the footer. This site does not include one because the details must come from the owner.
