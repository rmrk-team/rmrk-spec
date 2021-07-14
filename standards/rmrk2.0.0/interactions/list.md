# LIST

A LIST interaction lists an NFT as available for sale. The NFT can be instantly purchased. A listing
can be canceled, and is automatically considered canceled when a [BUY](./buy.md) is executed on top
of a given LIST.

You can only LIST an existing NFT (one that has not been [CONSUMEd](consume.md) yet).

An NFT that has another NFT as its owner CAN NOT be LISTed. An NFT-owned NFT must first be sent to
an account before being LISTed.

Selling an NFT which owns other NFTs will also transfer the ownership of those child NFTs. This is
bundling logic at play, allowing the trade of fully equipped game characters or even just simple
bundles (sets) of NFTs (e.g. Kanaria armor set and similar).

## Standard

The format of a LIST interaction is `0x{bytes(rmrk::LIST::{version}::{id}::{price})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).
- `price` is the listing price of the item, expressed in plancks (lowest denomination available on
  chain), e.g. `10000` for `1 KSM`. Alternatively, `price` can also be `0` which cancels the
  listing.

## Effects

An NFT's LIST is canceled by setting the price to `0`, or by the NFT being [sold](buy.md),
[sent](send.md), or [consumed](consume.md). While these actions will **NOT** emit the appropriate
LIST cancel interaction, they are **IMPLIED** to cancel a listing.

## Examples

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

> Note: submitting a new LIST with a different price changes the listing price.

Assuming we change our mind later on and decide to keep the NFT, we can cancel the listing by
setting price to 0:

```
rmrk::LIST::2.0.0::5105000-0aff6865bed3a66b-VALHELLO-POTION_HEAL-00000001::0
```
