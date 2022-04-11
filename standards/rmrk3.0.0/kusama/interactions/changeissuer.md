# CHANGEISSUER interaction (Kusama)

This document describes Kusama-specific examples and caveats for the [CHANGEISSUER](../../abstract/interactions/changeissuer.md) interaction.  See the [Abstract](../../abstract/interactions/changeissuer.md) for full specs.

## Standard (Kusama)

The format of a CHANGEISSUER interaction is
`0x{bytes(rmrk::CHANGEISSUER::{version}::{id}::{newissuer})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [base](../entities/base.md)'s ID or the [collection](../entities/collection.md)'s ID..
- `newissuer` is the address of the new owner, e.g.
  `CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp`

## Examples

### Base

If we want to change the owner of Base `base-575878273-kanaria_epic_birds` to
`HviHUSkM5SknXzYuPCSfst3CXK4Yg6SWeroP6TdTZBZJbVT`, we would compose:

```
rmrk::CHANGEISSUER::2.0.0::base-575878273-kanaria_epic_birds::HviHUSkM5SknXzYuPCSfst3CXK4Yg6SWeroP6TdTZBZJbVT
```

### Collection

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

If we want to change issuer to `HviHUSkM5SknXzYuPCSfst3CXK4Yg6SWeroP6TdTZBZJbVT`, we would compose:

```
rmrk::CHANGEISSUER::2.0.0::0aff6865bed3a66b-DLEP::HviHUSkM5SknXzYuPCSfst3CXK4Yg6SWeroP6TdTZBZJbVT
```
