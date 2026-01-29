# Malverk Touch UI Support (Mobile)

This note documents the minimal touch UI support added for the Malverk texture manager.

## Problem
Malverk’s texture selection UI relies on hover to show details and click-to-highlight to reveal Apply/Remove/Settings buttons. On mobile, hover isn’t available and CardArea highlighting is blocked for non-hand areas, so the UI becomes hard to use.

## Solution (minimal, PR-friendly)
We added small overrides in `utils/ui.lua` to map touch gestures to existing UI flows:

- **Single tap** on a texture pack card now toggles highlight, which shows Apply/Remove/Settings.
- **Long press** on a texture pack card now triggers hover, which shows the detail tooltip.

## Code Changes
File: `Malverk/utils/ui.lua`

Added:
- `Card:single_tap()` override for cards with `params.texture_pack` or `params.texture_priority`
- `Card:can_long_press()` override for those cards

Both delegate to the existing behavior for non-texture-pack cards, so mouse/controller behavior is unchanged.

## Notes
- No changes to mouse hover or click behavior.
- No changes to Malverk’s core logic or data model.
- Designed to be safe for upstream PRs (isolated and minimal).

