# UNEQUIP

Unequips an owned and equipped [NFT](../entities/nft.md) from a slot on its parent.

You can only UNEQUIP an existing NFT (one that has not been [CONSUMEd](consume.md) yet). You can
only UNEQUIP an NFT that is equipped.

## Standard

The format of an UNEQUIP interaction is `0x{bytes(rmrk::UNEQUIP::{version}::{id})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).

## Examples

Here's how we unequip a piece of armor. We do not need the parent NFT's ID because this NFT can only
be owned by one other NFT at any given time. We do not need to define a slot because the NFT can
only be equipped into a single slot on its owner at any given time, so this information is implied.

```
rmrk::UNEQUIP::2.0.0::5105000-0aff6865bed3a66b-DLEP-ARMOR-0000000000000001
```

The `children` record of a parent NFT will thus change from something like

```js
"children": {
    "5105000-0aff6865bed3a66b-DLEP-ARMOR-0000000000000001": "some_base_id.base_slot"
},
```

to

```js
"children": {
    "5105000-0aff6865bed3a66b-DLEP-ARMOR-0000000000000001": ""
}
```
