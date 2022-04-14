# SETPROPERTY interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [SETPROPERTY](../../abstract/interactions/setproperty.md) interaction.  See the [Abstract](../../abstract/interactions/setproperty.md) for full specs.

Properties in Substrate are set (for collections or NFTs) with the `set_property` extrinsic in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.  They are maintained in the Properties storage in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.  

## Standard (Substrate)
`mint_nft` takes the following arguments:
- `collection_id`: Collection ID to set the property for (or the collection ID of the NFT setting the property for)
- `maybe_nft_id`: NFT ID of the NFT to set the property for (optional)
- `key`: key of the property
- `value`: value of the property

## Caveats (Substrate)
There are no known caveats related to SETPROPERTY in Substrate.
