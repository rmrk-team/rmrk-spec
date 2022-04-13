# CREATE interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [CREATE](../../abstract/interactions/create.md) interaction.  See the [Abstract](../../abstract/interactions/create.md) for full specs.

Collections in Substrate are created with the `create_collection` extrinsic in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.

## Standard (Substrate)
`create_collection` accepts the following parameters:
- `metadata`: metadata to be stored on-chain
- `max`: maximum number of NFTs allowed to be created in this collection (optional)
- `symbol`: user-defined symbol for collection (e.g. "KANBIRDS")

## Caveats (Substrate)
Collection ID in Substrate is auto-incremented (starting at 0) and assigned upon creation.
