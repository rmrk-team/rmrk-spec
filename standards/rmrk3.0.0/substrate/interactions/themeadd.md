# THEMEADD interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [BUY](../../abstract/interactions/themeadd.md) interaction.  See the [Abstract](../../abstract/interactions/themeadd.md) for full specs.

Themes in Substrate are set (for a base) with the `theme_add` extrinsic in the [rmrk-equip](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-equip/src/lib.rs) pallet.  They are maintained in the Themes storage in the [rmrk-equip](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-equip/src/lib.rs) pallet.  `Theme` is defined as a [trait](https://github.com/rmrk-team/rmrk-substrate/blob/main/traits/src/theme.rs)

## Standard (Substrate)
`theme_add` takes the following arguments:
- `base`: base to add the theme for
- `theme`: `Theme` to add to the base, where `Theme` contains a `name` and a `properties`, which is a Vec of `ThemeProperties`, which contain a `key`, `value`, and `inherit` (an optional bool)

## Caveats (Substrate)
There are no known caveats related to THEMEADD in Substrate.