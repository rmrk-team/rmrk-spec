# CHANGEISSUER interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [CHANGEISSUER](../../abstract/interactions/changeissuer.md) interaction.  See the [Abstract](../../abstract/interactions/changeissuer.md) for full specs.

Collections in Substrate can have their issuer changed with the `change_issuer` extrinsic in the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.

## Standard (Substrate)
`change_issuer` accepts the following parameters:
- `collection_id`: Collection ID of the collection to change the issuer of
- `new_issuer`: account that will become the new issuer

## Caveats (Substrate)
At this time, RMRK Substrate pallets don't support CHANGEISSUER for Base.
TODO: update this when https://github.com/rmrk-team/rmrk-substrate/issues/118 is fixed (CHANGEISSUER currently only works for Collection, not base)