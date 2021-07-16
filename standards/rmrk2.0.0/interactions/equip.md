# EQUIP

Equips an owned [NFT](../entities/nft.md) into a slot on its parent, or unequips it.

You can only EQUIP an existing NFT (one that has not been [CONSUMEd](consume.md) yet). You can only
EQUIP an NFT into its immediate parent. You cannot equip across ancestors, or even across other
NFTs. You can only unequip an equipped NFT.

You can equip/unequip a non-transferable NFT. As an example, putting a helmet on or taking it off
does not change the ownership of the helmet.

You can only equip a [non-pening](accept.md) child NFT.

## Standard

The format of an EQUIP interaction is `0x{bytes(rmrk::EQUIP::{version}::{id}::{baseslot})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).
- `baseslot` is the base-namespaced slot into which the NFT is to be equipped. If unequipping, a
  falsy value like an empty string or null should be provided.

## Examples

Here's how we equip a piece of armor. We do not need the parent NFT's ID because this NFT can only
be owned by one other NFT at any given time, and it is _that_ NFT's base slots we are referencing.

```
rmrk::EQUIP::2.0.0::5105000-0aff6865bed3a66b-DLEP-ARMOR-00000001::base_1.slot_1
```

The `children` record of a parent NFT will thus change from something like

```json
"children": [
  {
    "id": "5105000-0aff6865bed3a66b-DLEP-DL15-00000002",
    "equipped": "",
    "pending": false,
  },
];
```

to

```js
"children": [
  {
    "id": "5105000-0aff6865bed3a66b-DLEP-DL15-00000002",
    "equipped": "base_1.slot_1",
    "pending": false,
  },
];
```

To unequip, we provide a falsy value (empty string, null) as the base slot:

```
rmrk::EQUIP::2.0.0::5105000-0aff6865bed3a66b-DLEP-ARMOR-00000001::
```

The resulting children property is:

```js
"children": [
  {
    "id": "5105000-0aff6865bed3a66b-DLEP-DL15-00000002",
    "equipped": "",
    "pending": false,
  },
];
```

### Baseslot Explained

Parts on a [base](entities/base.md) and resources on an [nft](entities/nft.md) reference a property
called `slot`.

Suppose we have an NFT A with resource `base_a` which contains slot `slot_1`. Now suppose we
[RESADD](interactions/resadd.md) a new resource, `base_b`, onto this NFT. Suppose that `base_b` also
has a slot called `slot_1`. We now have NFT A with two resources, each of which has `slot_1`.
Suppose now that we have an NFT which has a resource like this:

```json
  {
      "id": "V1i6B",
      "src": "hash-of-metadata-containing-guest-bird-art",
      "slot": "slot_1"
  },
```

If we now [SEND](interactions/send.md) this NFT into NFT A and issue the
[EQUIP](interactions/equip.md) command, the renderer would not know which slot to put the NFT's
resource into as a layer: `base_b` or `base_a`. Therefore, a `base` and `slot` namespaced
combination is necessary: `base_a.slot_1` vs `base_b.slot_1`.
