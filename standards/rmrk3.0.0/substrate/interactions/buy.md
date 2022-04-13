# BUY interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [BUY](../../abstract/interactions/buy.md) interaction.  See the [Abstract](../../abstract/interactions/buy.md) for full specs.

NFTs in Substrate are purchased with the `buy` extrinsic in the [rmrk-market](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-market/src/lib.rs) pallet.

## Standard (Substrate)
`buy` takes the following parameters:
- `collection_id`: Collection ID of the NFT to burn
- `nft_id`: NFT ID of the NFT to burn
- `amount`: purchase price (optional)

## Caveats (Substrate)
`amount` is a parameter used for safety, to ensure a NFT is only purchased at or below a certain price.
