⏺ Phase: 1
  Markets: US
  Language: English
  Ad/Privacy requirement: Current config
  Notes: Home market, launch here first
  ────────────────────────────────────────
  Phase:
  Markets: India
  Language: English
  Ad/Privacy requirement: Current config
  Notes: 2nd largest by downloads, English UI works, low CPMs
  ────────────────────────────────────────
  Phase:
  Markets: Canada
  Language: English
  Ad/Privacy requirement: Current config
  Notes: PIPEDA allows implied consent, high CPMs
  ────────────────────────────────────────
  Phase:
  Markets: Australia
  Language: English
  Ad/Privacy requirement: Current config
  Notes: No GDPR-style consent needed
  ────────────────────────────────────────
  Phase:
  Markets: New Zealand
  Language: English
  Ad/Privacy requirement: Current config
  Notes: Same as Australia
  ────────────────────────────────────────
  Phase:
  Markets: Philippines
  Language: English
  Ad/Privacy requirement: Current config
  Notes: English widely used, moderate DPA enforcement
  ────────────────────────────────────────
  Phase:
  Markets: Singapore
  Language: English
  Ad/Privacy requirement: Current config
  Notes: English official language, high spending
  ────────────────────────────────────────
  Phase:
  Markets: Malaysia
  Language: English
  Ad/Privacy requirement: Current config
  Notes: English widely understood
  ────────────────────────────────────────
  Phase: 2
  Markets: South Korea
  Language: Korean
  Ad/Privacy requirement: Current config
  Notes: Strong music ed culture, PIPA policy ready
  ────────────────────────────────────────
  Phase:
  Markets: Japan
  Language: Japanese
  Ad/Privacy requirement: Current config
  Notes: High-quality market, APPI policy ready
  ────────────────────────────────────────
  Phase:
  Markets: Vietnam
  Language: Vietnamese
  Ad/Privacy requirement: Current config
  Notes: Growing market, PDPD policy ready
  ────────────────────────────────────────
  Phase: 3
  Markets: EU bloc (Germany, France, Italy, Spain, Netherlands, Poland, Sweden, Austria,
    Belgium, etc.)
  Language: Local varies, English passable for tool apps
  Ad/Privacy requirement: UMP consent flow + ATT
  Notes: GDPR — all 27 member states as one block
  ────────────────────────────────────────
  Phase:
  Markets: UK
  Language: English
  Ad/Privacy requirement: UMP consent flow + ATT
  Notes: UK GDPR, but no localization needed
  ────────────────────────────────────────
  Phase:
  Markets: Switzerland
  Language: English passable
  Ad/Privacy requirement: UMP consent flow
  Notes: nDSG (GDPR-aligned since 2023)
  ────────────────────────────────────────
  Phase:
  Markets: Norway
  Language: English passable
  Ad/Privacy requirement: UMP consent flow + ATT
  Notes: EEA member, GDPR applies
  ────────────────────────────────────────
  Phase:
  Markets: Brazil
  Language: Portuguese
  Ad/Privacy requirement: UMP consent flow
  Notes: LGPD (GDPR-like, active enforcement)
  ────────────────────────────────────────
  Phase:
  Markets: Thailand
  Language: Thai
  Ad/Privacy requirement: UMP consent flow
  Notes: PDPA active since 2022, GDPR-like
  ────────────────────────────────────────
  Phase:
  Markets: Turkey
  Language: Turkish
  Ad/Privacy requirement: UMP consent flow
  Notes: KVKK mirrors GDPR
  ────────────────────────────────────────
  Phase: 4
  Markets: Indonesia
  Language: Bahasa Indonesia
  Ad/Privacy requirement: UMP consent flow
  Notes: Large population, low CPMs
  ────────────────────────────────────────
  Phase:
  Markets: Mexico
  Language: Spanish
  Ad/Privacy requirement: Current config possible
  Notes: LFPDPPP is lighter, but Spanish needed for adoption
  ────────────────────────────────────────
  Phase:
  Markets: Colombia/Argentina/Chile
  Language: Spanish
  Ad/Privacy requirement: Current config possible
  Notes: Growing LATAM markets
  ────────────────────────────────────────
  Phase:
  Markets: Saudi Arabia
  Language: Arabic (RTL)
  Ad/Privacy requirement: Consent flow
  Notes: PDPL 2023, RTL layout work needed
  ────────────────────────────────────────
  Phase:
  Markets: UAE
  Language: Arabic/English
  Ad/Privacy requirement: Consent flow
  Notes: English works for expat-heavy population
  ────────────────────────────────────────
  Phase:
  Markets: Taiwan
  Language: Traditional Chinese
  Ad/Privacy requirement: Current config
  Notes: PDPA moderate
  ────────────────────────────────────────
  Phase: Skip
  Markets: China
  Language: Mandarin
  Ad/Privacy requirement: Completely different stack
  Notes: No Google Play, no AdMob. Requires local app stores (Huawei/Xiaomi/Tencent), local ad
    SDK, ICP license. Essentially a separate product.
  ────────────────────────────────────────
  Phase:
  Markets: Russia
  Language: Russian
  Ad/Privacy requirement: Sanctions/legal complexity
  Notes: Roskomnadzor requirements, payment sanctions, high risk

  Key takeaway by effort level:

  - Phase 1 is 8 markets for zero additional work — just check the boxes in App Store Connect
  and Google Play Console
  - Phase 2 is 3 markets for i18n work only (the LOCALIZE.md plan)
  - Phase 3 is ~35+ markets behind one technical gate (UMP consent flow) — UK alone justifies
  this since it needs no localization
  - Phase 4 is diminishing returns requiring per-market localization
