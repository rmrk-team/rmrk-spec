# SETPRIORITY interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [SETPRIORITY](../../abstract/interactions/setpriority.md) interaction.  See the [Abstract](../../abstract/interactions/setpriority.md) for full specs.

Priorites in Substrate are set with the `set_priority` extrinsic in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.  They are maintained in the Priorities storage in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.  

## Standard (Substrate)
`mint_nft` takes the following arguments:
- `collection_id`: Collection ID of the NFT to send
- `nft_id`: NFT ID of the NFT to send
- `priorities`: the Vec of resources to set as priority

## Caveats (Substrate)
Priorites currently have no error checking on `priorities`, see https://github.com/rmrk-team/rmrk-substrate/issues/123
