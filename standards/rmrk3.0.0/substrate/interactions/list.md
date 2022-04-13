# LIST interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [LIST](../../abstract/interactions/list.md) interaction.  See the [Abstract](../../abstract/interactions/list.md) for full specs.

NFTs in Substrate are listed for sale and unlisted with the `list` and `unlist` extrinsics in the [rmrk-market](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-market/src/lib.rs) pallet.  Listed NFT data is maintained in the ListedNfts storage in the [rmrk-market](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-market/src/lib.rs) pallet.

## Standard (Substrate)
`list` takes the following parameters:
- `collection_id`: Collection ID of the NFT to list
- `nft_id`: NFT ID of the NFT to list
- `amount`: listing price
- `expires`: block number at which the listing expires (optional)

`unlist` takes the following parameters:
- `collection_id`: Collection ID of the NFT to unlist
- `nft_id`: NFT ID of the NFT to unlist

## Caveats (Substrate)
There are no known caveats related to LIST in Substrate.
