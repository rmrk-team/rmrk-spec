# EQUIP interaction (Kusama)

This document describes Kusama-specific examples and caveats for the [EQUIP](../../abstract/interactions/equip.md) interaction.  See the [Abstract](../../abstract/interactions/equip.md) for full specs.

## Standard (Kusama)

The format of an EQUIP interaction is `0x{bytes(rmrk::EQUIP::{version}::{id}::{baseslot})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).
- `baseslot` is the base-namespaced slot into which the NFT is to be equipped. If unequipping, a
  falsy value like an empty string or null should be provided.

## Examples (Kusama)

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
