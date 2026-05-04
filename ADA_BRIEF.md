# Sentral Website — ADA / WCAG Correction Brief

**For:** WordPress development team
**From:** Sentral (Jessica Cheshire, JCheshire@sentral.com)
**Companion to:** `Sentral_WP_Developer_Brief.docx`
**Target compliance:** WCAG 2.2 Level AA (with AAA where noted)

This document supplements the main developer brief with accessibility
requirements derived from an audit of the static HTML prototype. The
prototype's design language (Cormorant + DM Sans on cream/black, gold
accents) is correct and should be preserved; the issues below are about
**how** those colors and sizes are applied, not the palette itself.

A temporary `overrides.css` is included in the prototype repo to
illustrate what the corrected version should look like in the browser.
Use it as a visual reference when implementing in WordPress.

---

## 1. Color contrast — must hit WCAG AA on every text/background pair

The prototype uses three foreground colors against the cream body
background that fail WCAG AA. Recreate the design with the corrected
values below.

| Token | Hex | Used for | Cream contrast | Status | Replacement |
|---|---|---|---|---|---|
| `--gold` | `#c8b89a` | Eyebrows, italic accents, captions, decorative text | **1.76 : 1** | ❌ Fail AA | Use `#5e4a2c` (bronze) — **7.10 : 1**, AAA |
| `--lt` | `#9a9490` | Filter labels, secondary captions, badge text | **2.68 : 1** | ❌ Fail AA | Use `#6b6560` (`--mid`) — **5.14 : 1**, AA |
| `--mid` | `#6b6560` | Body paragraphs | **5.14 : 1** | ✅ AA | Keep, but consider darkening to `#3a3633` for AAA on body text |

**Targets to enforce in the WordPress theme:**
- Normal body text (< 18 pt / < 24 px): minimum **4.5 : 1**
- Large text (≥ 18 pt / ≥ 24 px or ≥ 14 pt bold): minimum **3 : 1**
- UI components & graphical objects: minimum **3 : 1**
- Aim for **AAA (7 : 1)** on body paragraphs in legal/long-form pages
  (Privacy, Terms).

**Brand-color caveat:** the gold `#c8b89a` is part of the approved
Sentral palette and remains valid as a *decorative* color on dark
backgrounds (where it passes against `#141414` at ~7.4 : 1). It must
NOT be used as text on cream. When an italic gold accent is desired
on a cream section, switch to the darker bronze `#5e4a2c`.

---

## 2. Hero typography over photo backgrounds

**Problem in the prototype:** every photo-hero page (Stay, Partner,
Experience, Careers, Group Business, SentralPlus, Home) places light
typography directly on a varied photo. Wherever the photo is
light-toned (skies, bright interiors), the headline becomes
unreadable.

**Required treatment for every photo hero:**

1. A persistent dark gradient scrim on the side where text sits, so
   contrast does not depend on what the photographer happened to
   capture. Recommended:
   ```css
   linear-gradient(
     to right,
     rgba(0,0,0,0.55) 0%,
     rgba(0,0,0,0.30) 45%,
     rgba(0,0,0,0.00) 100%
   );
   ```
2. A subtle text-shadow on the headline as belt-and-suspenders:
   `text-shadow: 0 2px 14px rgba(0,0,0,0.55);`
3. Subtitles/eyebrows on dark hero sections: minimum **70%** white,
   not 30–40% as in the prototype.

The headline must remain readable when the user provides a high-key
hero image with no dark areas — verify with a pure-white test image.

---

## 3. Minimum font size

**Problem in the prototype:** the Experience page has body text at
`0.42rem` (≈ 6.7 px). Several pages have labels at `0.52–0.58rem`
(≈ 8–9 px). These are unreadable for users with normal vision and
fail accessibility heuristics for low-vision users.

**Floor for the rebuild:**
- **Body / paragraph text:** minimum **16 px** (`1rem`).
- **Captions, labels, eyebrows, badges:** minimum **12 px** (`0.75rem`).
- **Letter-spacing on uppercase labels:** ≤ 0.18 em (the prototype
  uses up to 0.28 em on tiny fonts, which compounds illegibility).
- The site must scale gracefully when the user sets browser zoom to
  200 % — no overflow, no clipped content (WCAG 1.4.4).

---

## 4. Semantic structure & landmarks

The prototype is missing several landmarks. WordPress theme should
include:

| Requirement | Status in prototype | Required |
|---|---|---|
| `<html lang="en">` | ✅ Present | Keep |
| Single `<h1>` per page | ✅ Present | Keep |
| `<main>` landmark wrapping primary content | ❌ Missing on all 11 pages | Add |
| "Skip to main content" link as first focusable element | ❌ Missing | Add |
| `<nav>` with `aria-label` for primary + footer nav | Partial | Standardize |
| `<footer>` with `<address>` for contact | Verify | Confirm |
| Heading hierarchy without skipped levels (H1→H2→H3, no H1→H4) | Mostly OK | Audit during build |

---

## 5. Images & non-text content

- **`partner-with-us.html`** has 2 of 6 `<img>` elements without an
  `alt` attribute. Every informational image must have meaningful
  `alt` text; decorative images must have `alt=""` (empty, not absent).
- Property thumbnails (Stay/Live grids) need `alt` text describing
  the property name + city, not just the filename.
- Map pins on the Live With Us map must be keyboard-focusable and
  announce the property name + city to screen readers (currently
  click-only via Leaflet defaults).
- The Mews booking widget on Stay With Us and the Leaflet map on
  Live With Us are third-party embeds — confirm with vendors that
  their widgets pass WCAG 2.2 AA, or replace.

---

## 6. Keyboard & focus

- **Visible focus indicators are required.** The prototype suppresses
  default outlines via the standard `*:focus { outline: none }` CSS
  reset (or browser defaults). Every interactive element must show a
  clear focus ring on `:focus-visible`. Recommended:
  ```css
  outline: 2px solid #c8b89a;
  outline-offset: 3px;
  ```
- Tab order must follow visual order top-to-bottom, left-to-right.
- The hamburger menu must be operable by keyboard (Enter/Space to
  open, Escape to close, Tab cycles through items, focus returns
  to the trigger on close).
- Floating "Book a Stay / Apply to live here" CTAs must not trap
  focus or sit above interactive content without being reachable.

---

## 7. Forms & interactive elements

- All form inputs must have associated `<label>` elements (not just
  placeholders — placeholders disappear when typing).
- The search input on Live With Us currently has only a placeholder
  for guidance — add a visible label or `aria-label`.
- Error messages must be programmatically associated with inputs
  (`aria-describedby`).
- Buttons must use `<button>`, not `<div>` with click handlers.
- The "Schedule a Tour" / "Book a Stay" / "Apply to live here" CTAs
  need accessible names that match their visible labels.

---

## 8. Motion & animation

- The Press page has an opacity / fade-in animation that does not
  reliably trigger; cards beyond the first row render invisible.
  Whatever animation pattern is chosen must:
  - Have a non-animated fallback (content is visible by default,
    animation enhances).
  - Respect `prefers-reduced-motion: reduce` — disable parallax,
    fades, and movement for those users.
  - Not delay content visibility by more than 5 seconds (WCAG 2.2.1).

---

## 9. Cookie / consent banners

- If a cookie banner is added in WordPress, it must:
  - Be focusable and operable by keyboard.
  - Not block essential page content from screen readers when
    dismissed.
  - Provide equally prominent "Reject all" and "Accept all" options
    (GDPR + CCPA — even though those don't directly apply to ADA,
    they're the same audience).

---

## 10. Testing checklist before launch

The WordPress build should pass all of the following before going
live:

- [ ] Automated scan with **axe DevTools** or **WAVE**: zero errors,
      zero contrast failures.
- [ ] Lighthouse Accessibility score ≥ 95 on every page.
- [ ] Manual keyboard traversal of every page — no traps, all
      interactive elements reachable.
- [ ] Screen reader smoke test with **VoiceOver** (macOS Safari) and
      **NVDA** (Windows Firefox/Chrome) on Home, Stay, Live, Partner.
- [ ] Browser zoom 200 % — no horizontal scrolling on body content,
      no clipping.
- [ ] Color-blindness simulation (Chrome DevTools → Rendering →
      Emulate vision deficiencies) on hero sections and CTAs.
- [ ] Spot-check `prefers-reduced-motion` and `prefers-color-scheme`
      behavior.

---

## Reference: corrected color tokens

```css
:root {
  /* Brand palette — preserved */
  --black:    #141414;
  --ink:      #1a1a1a;
  --cream:    #f5f3ef;
  --white:    #ffffff;
  --gold:     #c8b89a;  /* dark-bg use only */

  /* Accessibility-corrected text tokens for use ON CREAM */
  --text-body:     #3a3633;  /* AAA, ~12.6:1 */
  --text-muted:    #6b6560;  /* AA,  ~5.14:1 */
  --text-accent:   #5e4a2c;  /* AAA, ~7.10:1 — replaces gold-on-cream */

  /* Reserved for dark-bg use */
  --text-on-dark:        #ffffff;
  --text-on-dark-muted:  rgba(255, 255, 255, 0.78);  /* min 70% */

  /* Typography floors */
  --fs-min-body:   1rem;     /* 16px */
  --fs-min-caption: 0.75rem; /* 12px */
}
```

---

## Questions / sign-off

For questions on any of the above or to confirm specific token values
before implementation, contact JCheshire@sentral.com.

This brief should be read alongside the original
`Sentral_WP_Developer_Brief.docx` and the prototype repo at
https://github.com/Sentral2026/sentral-website.
