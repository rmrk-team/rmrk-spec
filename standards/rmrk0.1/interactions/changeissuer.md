# CHANGEISSUER

The CHANGEISSUER interaction allows a [collection](../entities/collection.md) issuer to change the
issuer field to another address. The original issuer immediately loses all rights to mint further
[NFT](../entities/nft.md)s inside that collection.

This is particularly useful when selling the rights to a collection's operation or changing the
issuer to a null address to relinquish control over it.

## Standard

The format of a CHANGEISSUER interaction is
`0x{bytes(rmrk::CHANGEISSUER::{version}::{id}::{newissuer})}`.

- `version` is the version of the standard used (e.g. `0.1`)
- `id` is the [collection](../entity/collection.md)'s ID.
- `newissuer` is the address of the new issuer, e.g.
  `CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp`

## Examples

Given a collection like:

```json
{
  "version": "RMRK0.1",
  "name": "Dot Leap Early Promoters",
  "max": 100,
  "issuer": "CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp",
  "symbol": "DLEP",
  "id": "0aff6865bed3a66b-DLEP",
  "metadata": "ipfs://ipfs/QmVgs8P4awhZpFXhkkgnCwBp4AdKRj3F9K58mCZ6fxvn3j"
}
```

If we want to change owner to `HviHUSkM5SknXzYuPCSfst3CXK4Yg6SWeroP6TdTZBZJbVT`, we would compose:

```
rmrk::CHANGEISSUER::0.1::0aff6865bed3a66b-DLEP::HviHUSkM5SknXzYuPCSfst3CXK4Yg6SWeroP6TdTZBZJbVT
```

Which is submitted as:

```
0x726d726b3a3a4348414e47454953535545523a3a302e313a3a306166663638363562656433613636622d444c45503a3a4876694855536b4d35536b6e587a59755043536673743343584b34596736535765726f50365464545a425a4a625654
```
