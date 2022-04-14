# LOCK interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [LOCK](../../abstract/interactions/lock.md) interaction.  See the [Abstract](../../abstract/interactions/lock.md) for full specs.

Collections in Substrate can be locked with the `lock_collection` extrinsic in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.

## Standard (Substrate)
`lock_collection    ` takes the following parameters:
- `collection_id`: Collection ID of the collection to lock

## Caveats (Substrate)
There are no known caveats related to LOCK in Substrate.
