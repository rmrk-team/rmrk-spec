# EQUIP

Equips an owned [NFT](../entities/nft.md) into a slot on its parent.

You can only EQUIP an existing NFT (one that has not been [CONSUMEd](consume.md) yet). You can only EQUIP an NFT into its immediate parent. You cannot equip across ancestors, or even across other NFTs.

## Standard

The format of an EQUIP interaction is `0x{bytes(rmrk::EQUIP::{version}::{id}::{baseslot})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).
- `baseslot` is the base-namespaced slot into which the NFT is to be equipped.
## Examples

Here's how we equip a piece of armor. We do not need the parent NFT's ID because this NFT can only be owned by one other NFT at any given time, and it is *that* NFT's base slots we are referencing.

```
rmrk::EQUIP::2.0.0::5105000-0aff6865bed3a66b-DLEP-ARMOR-0000000000000001::base_1.slot_1
```

### Baseslot

Parts on a [base](entities/base.md) and resources on an [nft](entities/nft.md) reference a property called `baseslot`.

Suppose we have an NFT A with resource `base_a` which contains slot `slot_1`.
Now suppose we [RESADD](interactions/resadd.md) a new resource, `base_b`, onto this NFT. Suppose that `base_b` also has a slot called `slot_1`.
We now have NFT A with two resources, each of which has `slot_1`.
Suppose now that we have an NFT which has a resource like this:

```json
  {
      "media": "hash-of-metadata-containing-guest-bird-art",
      "slot": "slot_1"
  },
```

If we now [SEND](interactions/send.md) this NFT into NFT A and issue the [EQUIP](interactions/equip.md) command, the renderer would not know which slot to put the NFT's resource into as a layer: `base_b` or `base_a`. Therefore, a `baseslot` namespaced combination is necessary: `base_a.slot_1` vs `base_b.slot_1`.