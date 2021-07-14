# CONSUME

The CONSUME interaction burns an [NFT](../entities/nft.md) for a specific purpose.

This is useful when NFTs are spendable like with in-game potions, one-time votes in DAOs, or concert
tickets.

You can only CONSUME an existing NFT (one that has not been [CONSUMEd](consume.md) yet) and only if
you own it. If another NFT owns the NFT, the owner check bubbles up until the real owner is found
and can be checked (see `rootowner` computed property of the [NFT](../entities/nft.md)).

## Standard

The CONSUME interaction is a
[batchAll call](https://polkadot.js.org/api/cookbook/tx.html#how-can-i-batch-transactions) of two
remarks.

- the first message, A is: `0x{bytes(rmrk::CONSUME::{version}::{id})}`
- the second message, B is `0x{bytes({reason})}`

The format of CONSUME is thus: `utility.batchAll(system.remark(A), system.remark(B))`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entities/nft.md#computed-fields).
- `reason` is the byte-encoded burn reason specific to the issuing application, e.g.
  `0x69726f6e6d616964656e2d32303230313232352d7a61677265622d6172656e61` for
  "ironmaiden-20201225-zagreb-arena". The reason is arbitrary and completely up to the application.
  Correct interpretation of the reason is the responsibility of the application.

## Effects

This interactions cancels any pending [LIST](list.md) on the NFT with this ID. It is equivalent to
having called LIST with a cancel on it. It also cancels any pending SEND, and immediately removes
this NFT from the list of a parent NFT's children, if any.

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

Assume we want to consume this item in the [Valhello](https://valhello.app) game to restore our
character's health, and that the game looks for a CONSUME reason formatted thusly:
`valhello::HEALWITH::{itemid}::{character_address}`

The item's computed ID is `5105000-0aff6865bed3a66b-VALHELLO-POTION_HEAL-00000001`.

Message A is thus:

```
rmrk::CONSUME::2.0.0::5105000-0aff6865bed3a66b-VALHELLO-POTION_HEAL-00000001
```

Message B is:

```
valhello::HEALWITH::5105000-0aff6865bed3a66b-VALHELLO-POTION_HEAL-00000001::CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp
```

The final submission is thus:

```
utility.batchAll(system.remark(0x726d726b3a3a434f4e53554d453a3a322e302e303a3a353130353030302d306166663638363562656433613636622d56414c48454c4c4f2d504f54494f4e5f4845414c2d30303030303030303030303030303031), system.remark(0x76616c68656c6c6f3a3a4845414c574954483a3a353130353030302d306166663638363562656433613636622d56414c48454c4c4f2d504f54494f4e5f4845414c2d303030303030303030303030303030313a3a43706a734c4443314a467972686d3366744339477334516f79726b484b685a4b744b37597147545246745461666770))
```

## How does an app detect a CONSUME?

The game would be listening for `system.remark` extrinsics that start with its desired prefix
(valhello, or `0x76616c68656c6c6f`). When such a remark is detected, the accompanying tool should
look inside the same block and find the other remark which describes the item. If both messages are
valid, the burn is applied and can have effect in-app.
