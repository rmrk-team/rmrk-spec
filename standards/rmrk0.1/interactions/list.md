# LIST

A LIST interaction lists an NFT as available for sale. The NFT can be instantly purchased. A listing
can be canceled.

## Standard

The format of a LIST interaction is `0x{bytes(rmrk::LIST::{version}::{id}::{price})}`.

- `version` is the version of the standard used (e.g. `0.1`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).
- `price` is the listing price of the item, expressed in plancks (lowest denomination available on
  chain), e.g. `1000000000000` for `1 KSM`. Alternatively, `price` can also be `cancel` which
  cancels the listing.

## Examples

Suppose we have the following NFT:

```json
{
  "collection": "0aff6865bed3a66b-VALHELLO",
  "name": "POTION_HEAL",
  "transferable": 1,
  "sn": "0000000000000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4"
}
```

Suppose we want to sell it for 0.01 KSM, or 10000000000 plancks.

We would compose an interaction such as:

```
rmrk::LIST::0.1::0aff6865bed3a66b-VALHELLO-POTION_HEAL-0000000000000001::10000000000
```

Which would be submitted as:

```
0x726d726b3a3a4c4953543a3a302e313a3a306166663638363562656433613636622d56414c48454c4c4f2d504f54494f4e5f4845414c2d303030303030303030303030303030313a3a3130303030303030303030
```

Assuming we change our mind later on and decide to keep the NFT, we can cancel the listing:

```
rmrk::LIST::0.1::0aff6865bed3a66b-VALHELLO-POTION_HEAL-0000000000000001::cancel
```

Or in bytes:

```
0x726d726b3a3a4c4953543a3a302e313a3a306166663638363562656433613636622d56414c48454c4c4f2d504f54494f4e5f4845414c2d303030303030303030303030303030313a3a63616e63656c
```
