# EQUIPPABLE interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [EQUIPPABLE](../../abstract/interactions/equippable.md) interaction.  See the [Abstract](../../abstract/interactions/equippable.md) for full specs.

A specific collection's Slot Part may want to be updated subsequent to creation.  This is done with the `equippable` extrinsic in the [rmrk-equip pallet](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-equip/src/lib.rs).

## Standard (Substrate)
`equippable` takes the following parameters:
- `base_id`: Base ID of the `SlotPart` to change
- `slot_id`: Slot ID of the `SlotPart` to change
- `equippables`: `EquippableList` to update the `SlotPart` to.  `EquippableList` is an enum of `All`, `Empty`, or `Custom<Vec<CollectionId>>`.

## Caveats (Substrate)
There are no known caveats related to EQUIPPABLE in Substrate.
