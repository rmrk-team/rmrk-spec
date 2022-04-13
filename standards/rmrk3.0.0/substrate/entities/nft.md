[Root](../)

# NFT Entity (Substrate)

This document describes Substrate-specific examples and caveats for [NFT](../../abstract/entities/nft.md).  See the [Abstract](../../abstract/entities/nft.md) for full specs.

NFTs in Substrate are defined in the RMRK pallet's [Nft trait](https://github.com/rmrk-team/rmrk-substrate/blob/main/traits/src/nft.rs).  Storage of NFTs is within the Nfts storage of the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet. The following extrinsics, all within the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet, are relevant to NFTs:
- `mint_nft`: extrinsic for creating NFTs
- `burn_nft`: extrinsic for burning an NFT
- `send`: extrinsic for sending an NFT to another account or another NFT
- `accept_nft`: extrinsic for accepting a pending NFT
- `reject_nft`: extrinsic for rejecting a pending NFT
- `add_resource`: extrinsic for adding a resource to an NFT
- `accept_resource`: extrinsic for accepting a pending resource for an NFT
- `remove_resource`: extrinsic for removing a resource from an NFT
- `accept_resource_removal`: extrinsic for accepting a pending resource removal from an NFT
- `set_priority`: extrinsic for reordering priorities of resources associated with an NFT


## NFT Standard Caveats (Substrate)
NFT ID in Substrate is auto-incremented (starting at 0) and assigned upon creation.
