# Korean Typography Override

This file overrides and extends the typography rules in SKILL.md for Korean-language projects.

## 1. Font Stack (Mandatory)

**Primary:** `Pretendard` (all weights 100–900).
**Monospace:** `Pretendard` for UI numbers, `JetBrains Mono` for code blocks only.

```css
font-family: 'Pretendard Variable', Pretendard, -apple-system, BlinkMacSystemFont, system-ui, sans-serif;
```

### Banned Fonts
* **Noto Sans KR** — BANNED. Metrics are looser than Pretendard, causing inconsistent vertical rhythm when mixed with Latin glyphs. Pretendard already includes full KS X 1001 coverage with tighter sidebearings.
* **Spoqa Han Sans** — Avoid. Pretendard supersedes it.

## 2. Word Break & Line Break

Apply `word-break: keep-all` globally on all Korean text containers. Korean line breaks must occur between words (어절), never mid-syllable.

```css
/* Tailwind utility */
.break-keep-all { word-break: keep-all; }
```

In Tailwind v4, use the built-in `break-keep-all` class. In v3, add the utility via `@layer utilities` in your CSS:

```css
@layer utilities {
  .break-keep-all {
    word-break: keep-all;
  }
}
```

**Every** text element rendering Korean content MUST carry `break-keep-all`. No exceptions.

## 3. Line Height (Leading)

Korean glyphs are taller than Latin and need tighter leading to avoid excessive whitespace between lines.

| Context | Tailwind Class | Value |
|---|---|---|
| Headlines (H1–H2) | `leading-tight` | 1.25 |
| Subheadings (H3–H4) | `leading-snug` | 1.375 |
| Body text | `leading-relaxed` | 1.625 |
| Captions / UI labels | `leading-normal` | 1.5 |

**Override rule:** The SKILL.md default of `leading-none` (1.0) for Display/Headlines is too tight for Korean — Hangul descenders and ascenders clip at line-height 1.0. Use `leading-tight` (1.25) as the Korean headline baseline.

## 4. Korean Headline Scale

Korean headlines require a smaller scale multiplier than Latin headlines because Hangul characters are visually denser (uniform square matrix vs. variable-width Latin).

| Breakpoint | Latin default (SKILL.md) | Korean override |
|---|---|---|
| Mobile (`< md`) | `text-4xl` (2.25rem) | `text-3xl` (1.875rem) |
| Desktop (`≥ md`) | `text-6xl` (3.75rem) | `text-5xl` (3rem) |
| Display / Hero | `text-7xl`+ | `text-6xl` (3.75rem) max |

**Rationale:** Hangul at `text-6xl` on mobile causes overflow and feels heavier than equivalent Latin text. Scale down by one step and compensate with `font-semibold` or `font-bold` for visual weight.

## 5. Letter Spacing (Tracking)

* **Headlines:** `tracking-tight` (−0.025em). Same as SKILL.md baseline — works well for Korean.
* **Body:** `tracking-normal` (0). Do NOT apply `tracking-wide` or `tracking-wider` to Korean body text. Hangul already has built-in internal spacing; extra tracking fractures syllable blocks visually.
* **All-caps Latin within Korean context:** `tracking-widest` is acceptable for short labels only (e.g., brand names, navigation items).

## 6. Mixed Script Rules

When Korean and Latin text coexist:

* Wrap Latin phrases in `<span lang="en">` for proper font fallback and hyphenation.
* Maintain a single half-width space between Hangul and Latin/numeric boundaries (no double spaces, no zero-width joiners).
* Numbers follow Pretendard's tabular figures: `font-variant-numeric: tabular-nums` (`tabular-nums` in Tailwind).

## 7. Integration with SKILL.md

This file **overrides** the following SKILL.md rules for Korean projects:

| SKILL.md Rule | This Override |
|---|---|
| Rule 1: `leading-none` for headlines | `leading-tight` (1.25) |
| Rule 1: `text-4xl md:text-6xl` for headlines | `text-3xl md:text-5xl` |
| Rule 1: Font recommendations (Geist, Outfit, etc.) | `Pretendard` as primary; Geist/Satoshi acceptable for Latin-only elements |

All other SKILL.md rules remain in full effect.
