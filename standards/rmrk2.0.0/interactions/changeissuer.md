# CHANGEISSUER

The CHANGEISSUER interaction allows a [Base](../entities/base.md) or
[Collection](../entities/collection.md) issuer to change the issuer value to another address. The
original issuer immediately loses all rights to further interact with the entity.

This is particularly useful when another team is taking control over a project, or if an address was
compromised.

## Standard

The format of a CHANGEISSUER interaction is
`0x{bytes(rmrk::CHANGEISSUER::{version}::{id}::{newissuer})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [base](../entities/base.md)'s ID or the [collection](../entities/collection.md)'s ID..
- `newissuer` is the address of the new owner, e.g.
  `CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp`

## Examples

### Base

If we want to change the owner of Base `kanaria-epic-birds` to
`HviHUSkM5SknXzYuPCSfst3CXK4Yg6SWeroP6TdTZBZJbVT`, we would compose:

```
rmrk::BASEOWNER::2.0.0::kanaria-epic-birds::HviHUSkM5SknXzYuPCSfst3CXK4Yg6SWeroP6TdTZBZJbVT
```

Which is submitted as:

```
0x726d726b3a3a424153454f574e45523a3a322e302e303a3a6b616e617269612d657069632d62697264733a3a4876694855536b4d35536b6e587a59755043536673743343584b34596736535765726f50365464545a425a4a625654
```

### Collection

Given a collection like:

```json
{
  "version": "RMRK2.0.0",
  "name": "Dot Leap Early Promoters",
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

Which is submitted as:

```
0x726d726b3a3a4348414e47454953535545523a3a322e302e303a3a306166663638363562656433613636622d444c45503a3a4876694855536b4d35536b6e587a59755043536673743343584b34596736535765726f50365464545a425a4a625654
```
