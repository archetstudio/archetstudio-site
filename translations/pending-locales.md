# Pending / Future Locale Translations

Translations submitted for locales **not yet officially supported** in `metrotune-v141.js`. When a locale becomes officially supported, copy its entries from here into the main translations file.

This file is human-curated; the merge scripts (`/tmp/merge-*.js` and similar) only touch officially-supported locales.

---

## zh-CN — Simplified Chinese (Mainland)

### `unique.card3.body`
> 点按任意细分节拍点即可静音。打造预设里没有的节奏型——这种细致程度，其他节拍器应用很少能做到。

---

## zh-TW — Traditional Chinese (Taiwan)

### `unique.card3.body`
> 點一下任一細分拍點即可靜音。打造預設沒有的節奏型——這種細膩程度，其他節拍器 App 很少做得到。

---

## Notes

- Add new entries above this line with the same `## locale-code` heading + `### key.path` subheading + blockquote format.
- Source English for each translation is in `metrotune.html` (find by `data-i18n="<key>"`).
- When promoting a locale to officially-supported, also add the standard set of keys (meta, hero, walkthrough, scenes, grid, free, footer, etc.) — not just the ones recorded here.
