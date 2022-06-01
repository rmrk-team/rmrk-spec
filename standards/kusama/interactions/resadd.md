# RESADD interaction (Kusama)

This document describes Kusama-specific examples and caveats for the [RESADD](../../abstract/interactions/resadd.md) interaction.  See the [Abstract](../../abstract/interactions/resadd.md) for full specs.

## Standard (Kusama)

The format of a RESADD interaction is
`0x{bytes(rmrk::RESADD::{version}::{id}::{html_encoded_json})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [NFT](../entities/nft.md)'s ID
- `html_encoded_json` is the html encoded resource, formatted according to the standard in
  [NFT](../entities/nft.md) under the Resources section

## Examples (Kusama)

Suppose we have an NFT with the ID `5105000-0aff6865bed3a66b-DLEP-DL15-00000001`.

Suppose it has no resources right now:

```json
{
  "collection": "0aff6865bed3a66b-DLEP",
  "symbol": "DL15",
  "transferable": 1,
  "sn": "00000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4"
}
```

We want to add the following resource to it:

```json
{
  "id": "V1i6B",
  "src": "hash-of-metadata-guest-bird-art-with-jetpack",
  "metadata": "hash-of-metadata-with-credits"
}
```

So we issue:

```txt
rmrk::RESADD::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::%7B%22id%22%3A%22V1i6B%22%2C%src%22%3A%22hash-of-metadata-guest-bird-art-with-jetpack%22%2C%22metadata%22%3A%22hash-of-metadata-with-credits%22%7D
```
