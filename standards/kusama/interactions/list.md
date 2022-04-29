# LIST interaction (Kusama)

This document describes Kusama-specific examples and caveats for the [LIST](../../abstract/interactions/list.md) interaction.  See the [Abstract](../../abstract/interactions/list.md) for full specs.

## Standard (Kusama)

The format of a LIST interaction is `0x{bytes(rmrk::LIST::{version}::{id}::{price})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).
- `price` is the listing price of the item

## Examples (Kusama)

Suppose we have the following NFT minted in block 5105000:

```json
{
  "collection": "0aff6865bed3a66b-VALHELLO",
  "name": "POTION_HEAL",
  "transferable": 1,
  "sn": "00000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4"
}
```

Suppose we want to sell it for 0.01 KSM, or 10000000000 plancks.

We would compose an interaction such as:

```
rmrk::LIST::2.0.0::5105000-0aff6865bed3a66b-VALHELLO-POTION_HEAL-00000001::10000000000
```

Assuming we change our mind later on and decide to keep the NFT, we can cancel the listing by
setting price to 0:

```
rmrk::LIST::2.0.0::5105000-0aff6865bed3a66b-VALHELLO-POTION_HEAL-00000001::0
```
