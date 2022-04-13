# DESTROY interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [DESTROY](../../abstract/interactions/destroy.md) interaction.  See the [Abstract](../../abstract/interactions/destroy.md) for full specs.

Collections in Substrate are destroyed with the `destroy_collection` extrinsic in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.

## Standard (Substrate)
`destroy_collection` takes the following parameters:
- `collection_id`: Collection ID of the collection to destroy

## Caveats (Substrate)
There are no known caveats related to DESTROY in Substrate.
