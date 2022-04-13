# EQUIP interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [EQUIP](../../abstract/interactions/equip.md) interaction.  See the [Abstract](../../abstract/interactions/equip.md) for full specs.

The state of Equippings is maintained in the Equippings storage in the [rmrk-equip pallet](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-equip/src/lib.rs).  An NFT can be equipped (or unequipped) using the `equip` extrinsic in the [rmrk-equip pallet](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-equip/src/lib.rs).

## Standard (Substrate)
`equip` takes the following parameters:
- `item`: NFT to equip, identified as (Collection ID, NFT Id)
- `equipper`: NFT that is equipping, identified as (Collection ID, NFT Id)
- `base`: Base ID referencing where the item will be equipped
- `slot`: Slot ID referencing where the item will be equipped

## Caveats (Substrate)
Calling the `equip` extrinsic on an already-equipped item will cause the item to be unequipped.
