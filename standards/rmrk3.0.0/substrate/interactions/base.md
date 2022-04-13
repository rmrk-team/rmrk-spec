# BASE interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [BASE](../../abstract/interactions/base.md) interaction.  See the [Abstract](../../abstract/interactions/base.md) for full specs.

A [Base](../../abstract/entities/base.md) in Substrate is defined in the RMRK pallet's [Base trait](https://github.com/rmrk-team/rmrk-substrate/blob/main/traits/src/base.rs).  

Bases are created with the `create_base` extrinsic in the [rmrk-equip](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-equip/src/lib.rs) pallet.

## Standard (Substrate)

The `create_base` extrinsic takes a `base_type` (e.g. "svg"), `symbol` (e.g. "KANBIRD-BASE"), and `parts`.  `parts` is a Vec of `PartType`s.  `PartType` is an enum with possibilities `FixedPart` or `SlotPart`.

`FixedPart` allows for the following inputs:
- `id`: user-chosen u32 identifier for the part
- `z`: depth index
- `src`: IPFS or HTTPS link

`SlotPart` allows for the following inputs:
- `id`: user-chosen u32 identifier for the part
- `equippable`: enum with possibilities `All`, `Empty`, or `Custom<Vec<CollectionId>>`
- `src`: IPFS or HTTPS link
- `z` (depth index), 

## Caveats (Substrate)

There are no known caveats related to base in Substrate.
