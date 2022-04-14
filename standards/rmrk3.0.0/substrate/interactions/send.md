# SEND interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [SEND](../../abstract/interactions/send.md) interaction.  See the [Abstract](../../abstract/interactions/send.md) for full specs.

NFTs in Substrate are sent with the `send` extrinsic in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.

## Standard (Substrate)
`mint_nft` takes the following arguments:
- `collection_id`: Collection ID of the NFT to send
- `nft_id`: NFT ID of the NFT to send
- `new_owner`: the recipient of the NFT, enum `AccountIdOrCollectionNftTuple` with options `AccountId` (root account) or `CollectionAndNftTuple` (another NFT)

## Caveats (Substrate)
There are no known caveats related to SEND in Substrate.
