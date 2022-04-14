# RESADD interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [RESADD](../../abstract/interactions/resadd.md) interaction.  See the [Abstract](../../abstract/interactions/resadd.md) for full specs.

Resources in Substrate are added with the `add_resource` extrinsic in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.

## Standard (Substrate)
`add_resource` takes the following arguments:
- `collection_id`: Collection ID of the NFT to add a resource to
- `nft_id`: NFT ID of the NFT to add a resource to
- `resource_id`: the resource symbol, user-chosen
- `base`: Base ID if the resource is non-composable and should be equipped to a slot (optional)
- `src`: link to src resource for non-composable resource, should be string beginning with https:// or ipfs:// (optional)
- `metadata`: link to metadata resource for non-composable resource, should be string beginning with https:// or ipfs:// (optional)
- `slot`: Slot ID if the resource is non-composable and should be equipped to a slot (optional)
- `license`: Description of license or link to license file (optional)
- `thumb`: link to thumbnail resource, should be string beginning with https:// or ipfs:// (optional)
- `parts`: vec of `PartId`, if the resource is composable

## Caveats (Substrate)
There are no known caveats related to RESADD in Substrate.
