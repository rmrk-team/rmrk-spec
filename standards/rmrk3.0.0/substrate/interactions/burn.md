# BURN interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [BURN](../../abstract/interactions/burn.md) interaction.  See the [Abstract](../../abstract/interactions/burn.md) for full specs.

NFTs in Substrate are burned with the `burn_nft` extrinsic in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.

## Standard (Substrate)
`burn_nft` takes the following arguments:
- `collection_id`: Collection ID of the NFT to burn
- `nft_id`: NFT ID of the NFT to burn

## Caveats (Substrate)
NFTs in Substrate are defined by a combination of Collection ID and NFT ID.  The NFT ID alone is not necessarily unique, so both are required for `burn_nft`.