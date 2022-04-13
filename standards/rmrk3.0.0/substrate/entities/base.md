[Root](../)

# Base Entity (Substrate)

This document describes Substrate-specific examples and caveats for [Base](../../abstract/entities/base.md).  See the [Abstract](../../abstract/entities/base.md) for full specs.

A [Base](../../abstract/entities/base.md) in Substrate is defined in the RMRK pallet's [Base trait](https://github.com/rmrk-team/rmrk-substrate/blob/main/traits/src/base.rs).  Storage of bases is within the Bases storage of the [rmrk-equip](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-equip/src/lib.rs) pallet.  Bases are created with the create_base extrinsic in the rmrk-equip same pallet.

## Caveats for Substrate

There are no known caveats for Substrate Base entity

#### `id` (string)

A Base ID in Substrate is an auto-incrementing u32, assigned upon creation.

#### `equippable` (array)

Equippables in Substrate are represented as an enum of `All`, `Empty`, or `Custom<Vec<CollectionId>>`.  `All` handles the wildcard case.  This is defined in the RMRK pallet's [Part trait](https://github.com/rmrk-team/rmrk-substrate/blob/main/traits/src/part.rs)

---

### Themes Example

#### On chain

On-chain themes in Substrate are defined in the RMRK pallet's [Theme trait](https://github.com/rmrk-team/rmrk-substrate/blob/main/traits/src/theme.rs).    Storage of themes is within the Themes storage of the [rmrk-equip](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-equip/src/lib.rs) pallet.  Themes are created with the theme_add extrinsic in the rmrk-equip same pallet.

## Interactions

- [THEMEADD](../interactions/themeadd.md) - add a theme
- [BASE](../interactions/base.md) - creates a base
- [EQUIPPABLE](../interactions/equippable.md) - changes the equippable collection set
- [CHANGEISSUER](../interactions/changeissuer.md) - changes the issuer
