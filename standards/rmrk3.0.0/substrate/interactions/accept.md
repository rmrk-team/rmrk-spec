# ACCEPT interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [ACCEPT](../../abstract/interactions/accept.md) interaction.  See the [Abstract](../../abstract/interactions/accept.md) for full specs.

Resources in Substrate are defined in the RMRK pallet's [Resource trait](https://github.com/rmrk-team/rmrk-substrate/blob/main/traits/src/resource.rs).  Resource storage is maintained in the [rmrk-equip](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-equip/src/lib.rs) pallet.  The ACCEPT operation is implemented in the rmrk-equip pallet, with the `accept_nft` extrinsic for accepting a pending NFT, and the `accept_resource` extrinsic for accpeting a pending resource.  Additionally, there is a `accept_resource_removal` for accepting a pending resource removal.

## Caveats

`accept_nft`, `accept_resource`, and `accept_resource_removal` are separate extrinsics.

## Example for ACCEPTing a Pending Resource

Accepting a pending resource requires passing a `collection_id`, `nft_id`, and `resource_id`.

## Example for ACCEPTing a Pending Child

Accepting a pending child NFT (an NFT sent directly from some other account or non-owned NFT *directly to* an NFT) requires a `collection_id`, `nft_id`, and `new_owner` where `new_owner` is an enum `AccountIdOrCollectionNftTuple`.  The possibilities for this enum are `AccountId(AccountId)` or `CollectionAndNftTuple(CollectionId, NftId)`.  Generally one would pass the latter, along with the identifying `collection_id` and `nft_id` for the destination NFT.
