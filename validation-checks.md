# W3C Validation Checks — kitchen-sink.html

Validator used: [validator.w3.org/nu](https://validator.w3.org/nu/)
File checked: `kitchen-sink.html`

---

## validator-1.png — Original state (pre-may-2026-update)

21 issues found. These are the baseline issues that existed before any 2026 updates.

| # | Type | Issue | Location |
|---|------|-------|----------|
| 1 | Warning | `role="banner"` is unnecessary for `<header>` | line 10 |
| 2 | Error | `h2` not allowed as child of `hgroup` | line 26 |
| 3 | Error | `h3` not allowed as child of `hgroup` | line 27 |
| 4 | Error | `h4` not allowed as child of `hgroup` | line 28 |
| 5 | Error | `h5` not allowed as child of `hgroup` | line 29 |
| 6 | Error | `h6` not allowed as child of `hgroup` | line 30 |
| 7 | Warning | Section lacks heading (`<section>` inside `<article>`) | line ~68 |
| 8 | Warning | Article lacks heading | line ~56 |
| 9 | Error | Bad value `datetime` for `type` on `<input>` (deprecated) | line ~271 |
| 10–18 | Info | Trailing slash on void `<col />` elements (×9) | lines 624–635 |
| 19 | Warning | `role="contentinfo"` is unnecessary for `<footer>` | line 803 |
| 20 | Error | `h1` is a top-level heading only — multiple `h1` elements on page | line ~25 |
| 21 | Error | `h3` follows `h1`, skipping heading level (all section headings were `h3`) | line ~20 |

---

## validator-2.png — After may-2026-update changes

Same 21 issues. The may-2026-update PR (`update/kitchen-sink-elements`) added new elements
(`dialog`, `menu`, `datalist`, `map/area`, `template`, `object`) but did not address any
structural validation issues. No new errors were introduced.

---

## Fixes applied (this session)

All issues resolved unless noted.

| # | Type | Fix Applied |
|---|------|-------------|
| 1 | Warning | Removed `role="banner"` from `<header>` |
| 2–6 | Error | Removed `<hgroup>` wrapper from h1–h6 demo; added a correct `<hgroup>` example (one `h2` + one `<p>` subtitle) |
| 7 | Warning | Added `<h4>Section inside article</h4>` to nested `<section>` |
| 8 | Warning | Added `<h3>Article example</h3>` to `<article>` |
| 9 | Error | Changed `type="datetime"` → `type="datetime-local"` (datetime is deprecated) |
| 10–18 | Info | Removed trailing slashes from all `<col />` → `<col>` (×9 instances) |
| 19 | Warning | Removed `role="contentinfo"` from `<footer>` |
| 20 | Note | The `h1` inside the Headings demo section is retained for visual reference. Having multiple `h1` elements is flagged by validators and not recommended for production pages. Acceptable here as a style/reference demonstration. |
| 21 | Error | Changed all section-level headings from `h3` → `h2`; subsection headings (Forms, Tables) from `h4` → `h3` |

---

## validator-3.png — After fixes applied

**1 warning remaining. Document checking completed.**

| # | Type | Issue | Status |
|---|------|-------|--------|
| 1 | Warning | Consider using `h1` as top-level heading only — or use `headingoffset` attribute. Demo `h1` at line 24. | See validator-4 |

---

## validator-4.png — After headingoffset fix

**Target: 0 issues. Document checking completed.**

The remaining `h1` warning was resolved by wrapping the h1–h6 demo elements in
`<section headingoffset="1">`. This tells the validator (and conforming tools) that heading ranks
inside that section are offset by 1, so the demo `h1` is treated as an `h2` in the document
outline — while the `h1` is still present in source for visual reference.

### Why `headingoffset` and not removing the `h1`

**Option 1 (viable but not chosen):** Remove the demo `h1` entirely and note that `h1` is
demonstrated by the page title. This is a clean fix with zero tooling caveats.

**Option 2 (chosen):** Use `headingoffset="1"` on a wrapping `<section>` around the demo
headings. This preserves the full h1–h6 visual demonstration while satisfying W3C validation.
Note: `headingoffset` is part of the HTML living standard but browser support is currently
limited — it is a semantic/outline hint for validators and assistive tools, not a visual change.

This file is W3C validated as-is. That is final.

---

## validator-5.png — After inline SVG data URIs added (pre-space-encoding fix)

**6 errors. Document checking completed.**

All errors share the same root cause: literal spaces inside `data:` URIs are not allowed. The SVG
markup embedded in each data URI contained unencoded spaces between XML attributes, in `viewBox`
values, in `polygon points` lists, and in label text.

| # | Type | Issue | Location |
|---|------|-------|----------|
| 1 | Error | Bad value for `src` on `img`: illegal character (space) in data URI | line 223 — figure img 200×300 |
| 2 | Error | Bad value for `src` on `img`: illegal character (space) in data URI | line 731 — standalone img 150×150 |
| 3 | Error | Bad value for `src` on `img`: illegal character (space) in data URI | line 740 — usemap img 150×150 |
| 4 | Error | Bad value for `srcset` on `source`: space broke URI boundary, descriptor parse failed | line 783 — picture source 240×300 |
| 5 | Error | Bad value for `src` on `img`: illegal character (space) in data URI | line 784 — picture img 120×150 |
| 6 | Error | Bad value for `data` on `object`: illegal character (space) in data URI | line 796 — object 100×100 |

**Fix applied:** Encoded all spaces within the five SVG data URIs as `%20`. This covers spaces
between XML attributes, `viewBox` coordinate spaces, `polygon points` list spaces, and label text
spaces (e.g. `200 x 300` → `200%20x%20300`). The 150×150 URI was shared by two elements (lines
731 and 740) and fixed in one pass.

**Target: 0 errors. Document checking completed.**
