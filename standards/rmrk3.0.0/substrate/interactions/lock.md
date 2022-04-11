# LOCK

The LOCK interaction allows a [Collection](../entities/collection.md) issuer to lock the `max` value
to the current number of elements, effectively making the collection permanently limited.

## Standard

The format of a LOCK interaction is `0x{bytes(rmrk::LOCK::{version}::{id})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [collection](../entities/collection.md)'s ID.

## Examples

Given a collection like:

```json
{
  "version": "RMRK2.0.0",
  "max": 100,
  "issuer": "CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp",
  "symbol": "DLEP",
  "id": "0aff6865bed3a66b-DLEP",
  "metadata": "ipfs://ipfs/QmVgs8P4awhZpFXhkkgnCwBp4AdKRj3F9K58mCZ6fxvn3j"
}
```

Supposing we have 5 items minted in this collection, if we want to LOCK it:

```
rmrk::LOCK::2.0.0::0aff6865bed3a66b-DLEP
```

The `max` value of this collection will now permanently be `5`. No more NFTs will be mintable from
this collection.
