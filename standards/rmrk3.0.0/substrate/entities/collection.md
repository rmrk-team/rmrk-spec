[Root](../)

# Collection Entity (Substrate)

A [Collection](../../abstract/entities/collection.md) defines how **collection** of NFTs are minted.  This document describes Substrate-specific examples and caveats for Collections.  See the [Abstract](../../abstract/entities/collection.md) for full specs.

## Collection Standard in Substrate

Collections in Substrate are defined in the RMRK pallet's [Collection trait](https://github.com/rmrk-team/rmrk-substrate/blob/main/traits/src/collection.rs).  Storage of collections is within the Collections storage of the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.  Collections are created with the create_collection extrinsic in the same rmrk-core pallet.

## Caveats

Collection ID in Substrate is auto-incremented (starting at 0) and assigned upon creation.

### Properties Format

Properties are extracted out of the Collections storage.  Properties are defined in the RMRK pallet's [Properties trait](https://github.com/rmrk-team/rmrk-substrate/blob/main/traits/src/property.rs).  Storage of properties is within the Properties storage of the [rmrk-core](https://github.com/rmrk-team/rmrk-substrate/blob/main/pallets/rmrk-core/src/lib.rs) pallet.  Properties are added with the set_property extrinsic in the same rmrk-core pallet.

## Interactions

- [CREATE](../interactions/create.md) - creates a new collection
- [CHANGEISSUER](../interactions/changeissuer.md) - changes issuer to another address
- [LOCK](../interactions/lock.md) - locks a collection's max number of NFTs to the current number of
  elements
