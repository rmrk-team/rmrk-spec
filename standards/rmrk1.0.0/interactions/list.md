# LIST

A LIST interaction lists an NFT as available for sale. The NFT can be instantly purchased. A listing
can be canceled, and is automatically considered canceled when a [BUY](./buy.md) is executed on top
of a given LIST.

You can only LIST an existing NFT (one that has not been [CONSUMEd](consume.md) yet).

## Standard

The format of a LIST interaction is `0x{bytes(RMRK::LIST::{version}::{id}::{price})}`.

- `version` is the version of the standard used (e.g. `1.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).
- `price` is the listing price of the item, expressed in plancks (lowest denomination available on
  chain), e.g. `1000000000000` for `1 KSM`. Alternatively, `price` can also be `0` which cancels the
  listing.

## Effects

An NFT's LIST is canceled by setting the price to `0`, or by the NFT being [sold](BUY.md),
[sent](SEND.md), or [consumed](CONSUME.md). While these actions will **NOT** emit the appropriate
LIST cancel interaction, they are **IMPLIED** to cancel a listing.

## Examples

Suppose we have the following NFT minted in block 5105000:

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
RMRK::LIST::1.0.0::5105000-0aff6865bed3a66b-VALHELLO-POTION_HEAL-0000000000000001::10000000000
```

Which would be submitted as:

```
0x524d524b3a3a4c4953543a3a312e302e303a3a353130353030302d306166663638363562656433613636622d56414c48454c4c4f2d504f54494f4e5f4845414c2d303030303030303030303030303030313a3a3130303030303030303030
```

> Note: submitting a new LIST with a different price changes the listing price.

Assuming we change our mind later on and decide to keep the NFT, we can cancel the listing by
setting price to 0:

```
RMRK::LIST::1.0.0::5105000-0aff6865bed3a66b-VALHELLO-POTION_HEAL-0000000000000001::0
```

Or in bytes:

```
0x524d524b3a3a4c4953543a3a312e302e303a3a353130353030302d306166663638363562656433613636622d56414c48454c4c4f2d504f54494f4e5f4845414c2d303030303030303030303030303030313a3a30
```
