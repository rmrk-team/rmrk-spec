# MINT interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [MINT](../../abstract/interactions/mint.md) interaction.  See the [Abstract](../../abstract/interactions/mint.md) for full specs.

NFTs in Substrate are minted with the `mint_nft` extrinsic in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.

## Standard (Substrate)
`mint_nft` takes the following arguments:
- `collection_id`: Collection ID of the NFT to mint
- `nft_id`: NFT ID of the NFT to burn
- `recipient`: the account to receive royalties (optional)
- `royalty`: amount of royalties as Permill, where 1_000_000=100%, 10_000=10% (optional)
- `metadata`: link to metadata (should be string beginning with https:// or ipfs://)

## Caveats (Substrate)
NFT ID in Substrate is auto-incremented (starting at 0) and assigned upon creation.
