# MINT

The MINT interaction creates an [NFT](../entities/nft.md) in/from a [class](../entities/nftclass.md).

## Standard

The format of a MINT interaction is the same as for [CREATE](create.md):
`0x{bytes(rmrk::MINT::{version}::{html_encoded_json}::recipient?)}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `html_encoded_json` is the full content of the NFT's json, html encoded
- `recipient` is an optional argument whih can be an NFT ID (which becomes a parent of this NFT), or an account, e.g. `CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp`. If omitted, the recipient is implied to be the minter. Otherwise, mints directly into another account or NFT.

## Examples

```json
{
  "nftclass": "0aff6865bed3a66b-DLEP",
  "symbol": "DL15",
  "name": "Dot Leap 15 Promo NFT",
  "transferable": 1,
  "sn": "0000000000000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4",
}
```

Becomes:

```
rmrk::MINT::2.0.0::%7B%22nftclass%22%3A%220aff6865bed3a66b-DLEP%22%2C%22symbol%22%3A%22DL15%22%2C%22name%22%3A%22Dot+Leap+15+Promo+NFT%22%2C%22transferable%22%3A1%2C%22sn%22%3A%220000000000000001%22%2C%22metadata%22%3A%22ipfs%3A%2F%2Fipfs%2FQmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4%22%7D
```

If we also want to mint it straight into someone's account:

```
rmrk::MINT::2.0.0::%7B%22nftclass%22%3A%220aff6865bed3a66b-DLEP%22%2C%22symbol%22%3A%22DL15%22%2C%22name%22%3A%22Dot+Leap+15+Promo+NFT%22%2C%22transferable%22%3A1%2C%22sn%22%3A%220000000000000001%22%2C%22metadata%22%3A%22ipfs%3A%2F%2Fipfs%2FQmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4%22%7D::CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp
```