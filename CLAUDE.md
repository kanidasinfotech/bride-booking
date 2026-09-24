# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-page marketing + booking-enquiry site for a bridal photography studio ("Blush & Gold" — placeholder brand). Everything lives in `index.html`: HTML, one inline `<style>`, one inline `<script>`. The single-file constraint is a requirement — do not split into separate CSS/JS files or add a build step, framework or package manager.

## Running

No build, lint or test tooling. Open `index.html` directly, or serve it:

```bash
python -m http.server 8321
```

Only external dependencies are Google Fonts (Cormorant Garamond, Jost) and Unsplash images loaded by URL (`images.unsplash.com/photo-<id>?auto=format&fit=crop&w=<n>&q=80`). When swapping images, verify the photo ID loads — the lightbox rewrites the `w=` param to 1600 for full-size views, so keep that param in gallery URLs.

## Architecture (index.html)

**CSS** — design tokens on `:root` (blush scale `--blush-50…600`, `--gold`/`--gold-light`/`--gold-dark`, `--ink`, `--muted`, `--serif`/`--sans`). Use the tokens rather than new hex values. Mobile-first: base styles are for phones, with two breakpoints — `min-width: 640px` (two-column form, 3-col gallery, 3-col contact) and `min-width: 900px` (inline desktop nav, 3-col packages, 4-col gallery, featured card scale). `.reveal` elements fade in via IntersectionObserver; `prefers-reduced-motion` disables this.

**JS** — one IIFE, ES5-style (`var`, `function`), no dependencies. Pieces:
- Header gets `.scrolled` after 40px; `.nav-open` on the header drives the mobile menu.
- Lightbox is delegated from `#gallery-grid`; gallery items are `<button>`s for keyboard access.
- Package card buttons carry `data-package="<value>"` which preselects `#package` in the form — values must match the `<option value>`s exactly.
- **Form validation** is driven by the `validators` object, keyed by input `id`. Each function takes the trimmed value and returns an error string or `undefined`. Each field needs matching markup: a `.field` wrapper, the input with that `id`, and `<span class="error-msg" id="<id>-error">`. Adding a field = add markup + a `validators` entry; submit, live re-validation, error display and reset all iterate `Object.keys(validators)` automatically. The form uses `novalidate`; all validation is in JS.
- On valid submit, data is collected via `FormData` and the success panel (`#success`) is filled with a summary. **There is no backend** — the `// Placeholder: send data to your backend` comment in the submit handler is where a real submission (fetch to an API / form service) belongs.

## Content placeholders

Prices, studio name, stats, contact email/phone/address and social links are placeholders awaiting real details.
