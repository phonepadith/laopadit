# Plan: Restore the LaoPadit palette on the Products surface

Status: proposed
Surface: `index.html` — `#products` section
Baseline commit: ab304f5

## Problem

The `#products` section introduces `#5BCB25` (SlimFit's brand green) into the
LaoPadit landing surface. That value is not defined in `:root` and is not used
anywhere else on the page. Every other decorative accent on this surface resolves
to one of the four declared tokens.

## Evidence

`index.html:14-21` declares the surface palette:

```
--pink: #ec4899;  --purple: #8b5cf6;  --orange: #f97316;  --cyan: #06b6d4;
```

All other `.mesh-bg` instances use a declared token:

| line | color | token |
| --- | --- | --- |
| 350 | `#8b5cf6` | `--purple` |
| 351 | `#ec4899` | `--pink` |
| 352 | `#06b6d4` | `--cyan` |
| 413 | `#8b5cf6` | `--purple` |
| 598 | `#ec4899` | `--pink` |
| 643 | `rgba(236,72,153)/rgba(139,92,246)` | `--pink`/`--purple` |
| **531** | **`#5BCB25`** | **none — off palette** |

All four `.service-icon` backgrounds in `#services` (lines 468, 481, 497, 508)
likewise resolve to `--pink`, `--purple`, `--orange`, `--cyan`.

The `#5BCB25` value reaches the rendered surface twice, both inside `#products`:
- line 531 — section `.mesh-bg` radial
- line 536 — SlimFit card decorative radial

## Correction

Replace both occurrences of `#5BCB25` with `#8b5cf6` (`--purple`).

`--purple` is the correct choice over the other three tokens: it is the token the
two other section-level `.mesh-bg` radials use (lines 350, 413), and the products
section sits between `#services` (pink radial, line 598 region) and the rest of
the page, so purple preserves the existing section-to-section alternation.

Leave `#06b6d4` on the rPPG card (line 546) and its step numbers unchanged — that
value is `--cyan`, a declared token already in use at line 352 and line 508.

Leave `img/slimfit-logo.svg` unchanged. The green inside the SlimFit lockup is that
product's own mark, not surface chrome, and the audit contract governs surface
accents only.

## Affected surfaces

`#products` only. No other section references `#5BCB25`.

## Verification

After the edit, `grep -c '5BCB25' index.html` must return `0` for `index.html`
(the string remains only inside `img/slimfit-logo.svg`, which is out of scope).
