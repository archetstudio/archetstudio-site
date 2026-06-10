# Archet Studio Site Operating Rules

You are assisting Archet Studio LLC with management of archetstudio.com — a static GitHub Pages site for a small mobile app studio. Follow these operating conventions and constraints.

## Project

- **Repo**: archetstudio/archetstudio-site (GitHub Pages, branch: main)
- **Working directory**: /Users/minyae/works/claudeProjects/ArchetStudioWeb/archetstudio-site
- **Stack**: HTML5, Tailwind CSS (static CDN/min build — NOT all dynamic classes available; verify class exists before relying on it; add custom CSS if missing e.g. flex-col-reverse, lg: variants)
- **Fonts**: Inter (body), Lato (logo wordmark)
- **Theme**: dark-only — bg-gray-900, text-gray-100/gray-400, blue-400/500 accents
- **Pages**: index.html (landing), metrotune.html (product page), metrotune/guide.html (user guide), terms.html, privacy/* (13 locales)

## Git & Identity (NON-NEGOTIABLE)

- Commit author: "Archet Studio" / 262100550+archetstudio@users.noreply.github.com
- NEVER add Co-Authored-By lines, NEVER mention Claude/AI/agents in commits
- NEVER include app names (e.g. "MetroTune") in commit messages — keep generic ("production deploy", "site update", etc.)
- Quick commands the user invokes verbatim:
  - **"commit and push"** OR **"squash and push"** → orphan reset to a single "production deploy" commit and force push:
    ```bash
    git add <changes> &&
    git checkout --orphan _squash &&
    git commit -m "production deploy" \
      --author="Archet Studio <262100550+archetstudio@users.noreply.github.com>" &&
    git branch -M main &&
    git push -f origin main
    ```
- Always force push after orphan reset (this is intentional, expected behavior)
- Confirm before destructive operations (deletes, force pushes outside the squash workflow, etc.)

## Contact Email Rule

- NEVER write the user's personal email (inpyo82@gmail.com) in any committed file. Use @archetstudio.com addresses only:
  - inquiry@archetstudio.com (general)
  - support@archetstudio.com (app support)
  - privacy@archetstudio.com (privacy/data deletion)

## Marketing & Copy Rules (CRITICAL)

Apply these rules to ALL customer-facing copy on metrotune.html, guide.html, privacy/* footers, and any landing-page text:

1. **No categorical absolutes**: never use "no other," "every," "only," "single," "unique," "always," "never" without verifying the claim is true. Most are provably false. Soften to "few other," "most apps don't," or drop the comparison entirely.
2. **No unsubstantiated UX claims**: avoid promises like "practice for hours without fatigue," "always perfectly in tune," "never miss a beat." Soften to "easier on the eyes for long sessions," "more accurate pitch detection," etc.
3. **No unverified UI descriptions**: don't ship descriptions of UI behavior you haven't confirmed by reading the code or testing. If unsure, OMIT the line and ASK the user before publishing.
4. **No narrowing the user segment without being asked**: default audience is "musicians" or "serious musicians" — do NOT pivot hero/tagline copy toward beginners, students, professionals, etc. unless explicitly directed.
5. **Quantitative claims need verification**: "17 sound profiles balanced to identical loudness" implies 100% match. If reality is ±1.5 dB, write "balanced to a common target" instead.

**Default rule**: before writing any sentence with the words "no other," "every," "always," "never," "only," "single," "unique" — pause and ask: "Do I know this is true? Can I cite the source?" If no, ASK the user.

## Decided Marketing Copy (use verbatim, don't rewrite)

- **Tagline**: "Practice Smarter. Play Better."
- **Subtitle/descriptor**: "Metronome meets Tuner."
- **Feature caption**: "HANDS-FREE CONTROL · REAL-TIME PITCH DETECTION"
- **Trademark**: use ™ (not ®) — legal for unregistered marks

## Privacy Policy Structure (IMPORTANT)

The site has 13 privacy policy files — these are NOT parallel translations of English. Each is a **jurisdiction-specific compliance document**:

| File | Jurisdiction |
|------|--------------|
| privacy/metrotune.html | English (x-default), CCPA/GDPR overview |
| privacy/metrotune-ko.html | Korea PIPA (제1조–제18조) |
| privacy/metrotune-ja.html | Japan APPI (第1条–第16条) |
| privacy/metrotune-vi.html | Vietnam Decree 13/2023 (Điều 1–14) |
| privacy/metrotune-th.html | Thailand PDPA B.E. 2562 (ข้อ 1–...) |
| privacy/metrotune-de.html | Germany GDPR/DSGVO |
| privacy/metrotune-es.html | Spain RGPD/LOPDGDD |
| privacy/metrotune-es-mx.html | Mexico LFPDPPP |
| privacy/metrotune-fr.html | France RGPD/CNIL |
| privacy/metrotune-it.html | Italy GDPR/Garante |
| privacy/metrotune-id.html | Indonesia UU PDP (UU 27/2022) |
| privacy/metrotune-pt-br.html | Brazil LGPD |
| privacy/metrotune-pt.html | Portugal RGPD/CNPD |

When ad SDKs, data flows, processors, or retention practices change, comprehensive updates are required across each locale's compliance disclosures (purposes, data items, processor/entrustment table, cross-border transfer table, retention, security, auto-collection, cookies/identifiers).

**Cross-border transfer pattern**: when there are N foreign data recipients, split the transfer section into N sub-tables, one per recipient, each with full columns (recipient, country, data items, purpose, transfer method, retention, contact).

**Processor/entrustment pattern**: add one row per processor. Tables typically use columns: Processor / Activity / Data items.

**Header version**: each file has an "Effective Date | Last Updated | Version" line at the top. Bump version + date on any substantive change. Some files have a separate "Current version" reference inside §12-equivalent — keep both in sync.

**Ad SDK proper nouns** (preserve verbatim, do NOT translate):
AdMob, Pangle, ByteDance, Vungle, Liftoff, Liftoff Mobile Inc., IDFA, IDFV, AAID, App Tracking Transparency, ATT, Google Mobile Ads SDK, Google UMP, NSPrivacyTracking, NSPrivacyTrackingDomains, FOREGROUND_SERVICE_MICROPHONE.

**Privacy policy URLs** (preserve verbatim):
- Google AdMob: https://policies.google.com/privacy
- Pangle: https://www.pangleglobal.com/privacy/policy
- Vungle: https://vungle.com/privacy/

**Locale-specific conventions**:
- **Thai**: use เรา (we/us), NOT บริษัท. Buddhist Era dates (2026 = พ.ศ. 2569). Formal ท่าน register. Use โปรด not กรุณา. ข้อ-numbered sections.
- **Korean**: 합니다 form. 제N조 numbered sections. Buddhist Era NOT used.
- **Japanese**: です/ます form. 第N条 numbered sections.
- **Spanish (Mexico)**: formal "usted" register, "recolectar/recolección" (NOT "recoger/recogida"), "computadora" not "ordenador," "aplicación" not "app" in formal contexts.
- **Spanish (Spain)**: formal "usted" register, RGPD terminology.
- **Portuguese (Brazil)**: "celular," "tela," "aplicativo," LGPD terminology.
- **Portuguese (Portugal)**: "telemóvel," "ecrã," "aplicação," "utilizador," RGPD/CNPD terminology, "recolher/recolha" not Brazilian "coletar/coleta."
- **German**: formal Sie register, GDPR/DSGVO terminology (Auftragsverarbeitung, Drittlandübermittlung, Verantwortlicher).
- **French**: formal vous register, RGPD/CNIL terminology (responsable du traitement, sous-traitant, transfert hors UE).
- **Italian**: formal Lei register, GDPR/Garante terminology (titolare, responsabile, interessato, trasferimento extra-UE).
- **Indonesian**: formal Anda register, UU PDP terminology (Pengendali Data Pribadi, Subjek Data Pribadi, Prosesor Data Pribadi).

## "What's New in vX.YY" Callout Format

When announcing a release on metrotune.html:

- Header includes "& IMPROVED" not just "NEW"
- Each bullet has bold name + "(new)" or "(improved)" tag + em-dash + concrete one-sentence description
- Pagination: ~4 visible bullets + "Show N more" expand affordance

## Guide Page Status Badges

After ANY edit to metrotune/guide.html, recheck and update `data-status` attributes on sidebar links: "stub" / "in-progress" / "complete." Drop the top-of-page work-in-progress banner once everything is "complete."

## Site Design Patterns (use these, don't reinvent)

- **Nav**: logo-nav.svg left, text links right, border-b border-gray-800
- **Cards**: bg-gray-800 border border-gray-700 rounded-xl p-6 (or p-5 for tighter)
- **Buttons**:
  - Primary: bg-gray-100 text-gray-900
  - Ghost: border border-gray-700
- **Status badges**: text-yellow-400 bg-yellow-400/10 border-yellow-400/20 rounded-full
- **Container max-width**: max-w-5xl (landing pages), max-w-3xl (privacy / docs)

## Brand Assets

- Horizontal logo (dark-theme native colors): images/logo-nav.svg
- Square logo: images/logo.svg
- App icon: images/icon.png, images/metrotune-icon.png
- OG image: images/adaptive-icon.png
- DO NOT apply CSS filter hacks — SVG has correct native colors

## i18n / Translations

- 13 officially supported locales: de, es, es-MX, fr, id, it, ja, ko, pt-BR, pt-PT, th, tr, vi (English uses HTML fallback, no "en" key)
- Translation source: translations/metrotune-v141.js
- For locales NOT yet officially supported (e.g. zh-CN, zh-TW), submit translations to translations/pending-locales.md — never to the main file
- Merge scripts at /tmp/merge-*.js handle batch updates
- Marketing copy uses data-i18n="<key>" attributes on metrotune.html

## app-ads.txt (Ad Network Authorization)

- File: app-ads.txt at site root
- Currently authorized:
  - google.com / pub-8090048526672531 / DIRECT (TAG: f08c47fec0942fa0)
  - vungle.com / 69f1b0687dbb2c94758f1258 / DIRECT (TAG: c107d686becd2d77)
  - pangleglobal.com / 5038381 / DIRECT
  - pubmatic.com / 161490 / RESELLER (Pangle's required line)
  - + 359 Vungle/Liftoff downstream reseller authorizations
- When a new ad network is added: add their DIRECT line with publisher ID + TAG-ID. Verify TAG-IDs (16 hex chars) — don't fabricate.
- Format: `<domain>, <publisher_id>, DIRECT|RESELLER[, <TAG-ID>]`

## Reddit Pixel & Smart Banner

- Reddit Pixel script at top of metrotune.html with bot filter (navigator.webdriver + UA regex). Don't remove without explicit ask.
- Uses event.preventDefault() + 250ms setTimeout before navigation to flush pixel HTTP request before page unload — preserve this pattern.
- Smart banner auto-redirect for Reddit visitors is currently PAUSED (commented out). Don't uncomment without confirming with user.

## Communication Style

- The user prefers terse, specific updates. No trailing summaries unless explicitly asked.
- Match response length to task complexity.
- For ambiguous instructions, ASK before publishing. Especially for marketing claims, privacy policy claims, or anything customer-facing.
- When dispatching long-running work, give a one-sentence "I'm doing X" before the tool call.
- Don't generate documentation files (*.md) or README files unless explicitly requested.

## Defaults to AVOID

- Don't add features, refactor, or introduce abstractions beyond what the task requires.
- Don't add error handling/fallbacks/validation for scenarios that can't happen.
- Don't add comments unless the WHY is non-obvious. Never write multi-paragraph docstrings.
- Don't include emojis unless explicitly requested.
- Don't generate "I'll flag for verification" copy. Either confirm and ship, or ask and wait.
- Don't translate proper nouns or product names.
- Don't change file structure (page layout, CSS, navigation, footer, contact emails, postal address) without an explicit ask.

## Verification Patterns

After editing translation/i18n content, run grep to verify:

- New SDK names / proper nouns appear as expected
- Version + date strings are updated consistently
- All three SDK URLs are present where expected
- HTML tag balance (`grep -c "<table" file` and `grep -c "</table>" file`)
- No remaining old version strings, dates, or removed SDK references

When in doubt, ask the user, especially for marketing claims, jurisdiction-specific privacy disclosures, ad-tech compliance, Git operations beyond the documented squash-and-push workflow, and anything touching legal/regulatory surface.
