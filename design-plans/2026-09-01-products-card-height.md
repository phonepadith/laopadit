# Plan: Remove `items-start` from the Products card grid

Status: proposed (awaiting selection)
Surface: `index.html` — `#products` section
Baseline commit: ab304f5

## Problem

The two cards in the products grid render at different heights. The SlimFit card
ends well above its rPPG sibling, leaving a visible ragged bottom edge that no
other card grid on this surface produces.

## Evidence

`index.html:539` is the only card grid on the surface that opts out of the default
stretch alignment:

| line | grid | alignment |
| --- | --- | --- |
| 390 | stats | default (stretch) |
| 435 | about cards | default (stretch) |
| 465 | services cards | default (stretch) |
| **539** | **products cards** | **`items-start`** |
| 605 | use-case cards | default (stretch) |
| 660 | footer columns | default (stretch) |

Line 415 uses `items-center`, but that grid holds a prose column and an image
column, not sibling cards, so it is not a card-grid precedent.

Tailwind is loaded from CDN and applies at runtime (`window.tailwind === true`),
and the grid is two-column from the `md` breakpoint up, so both cards are visible
side by side whenever the contradiction renders.

## Correction

Delete `items-start` from the class list at `index.html:539`.

The grid returns to `align-items: stretch`, matching the five other card grids, and
both cards render to equal height.

## Affected surfaces

`#products` only.

## Verification

At a viewport width of 1280px, the SlimFit and rPPG cards share the same bottom
edge. Below `md` the cards stack and the change has no effect.
