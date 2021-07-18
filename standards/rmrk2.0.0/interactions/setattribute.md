# SETATTRIBUTE

The `SETATTRIBUTE` interaction allows NFT owners to change certain mutable values on their NFTs.

The types of on-chain attributes susceptible to this interaction are: mutable, conditional, logic.

## Mutable

Attributes are mutable if they are defined to be mutable by the minter (see
[NFT On-chain Attributes](../entities/nft.md#on-chain-attributes)).

A mutable attribute is changed with a SETATTRIBUTE call:

```txt
rmrk::SETATTRIBUTE::2.0.0::{id}::{html_encoded_name}::{html_encoded_value}
```

Example. Given an NFT `5105000-0aff6865bed3a66b-DLEP-DL15-00000001` with an attribute (either
inherited from Collection or directly defined on NFT):

```json
{
  "mutable": true,
  "trait_type": "Hair color",
  "value": "blue"
}
```

We can change the color to red like so:

```txt
rmrk::SETATTRIBUTE::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::Hair%20color::blue
```

If the attribute was inherited, it appears as an NFT-level attribute on the NFT henceforth, since
the new value diverges from what was inherited from the Collection.

The attribute can also have the `mutator` property, and the `bubble` property:

```json
{
  "mutable": true,
  "trait_type": "Hair color",
  "value": "blue",
  "mutator": "owner",
  "bubble_on_equip": true
}
```

`mutator` can be `owner` (default) or `issuer`. This defines who can mutate this item's attribute:
the current owner of the NFT, or the issuer of the collection. `bubble_on_equip` defines whether
this attribute is local (default: `false`), or global (`true`), i.e. bubbles up to its parent.

Example of `mutator:owner` and `bubble_on_equip:true` is a name-change crystal where renaming a
`name` attribute on an NFT applies the new value to the `name` attribute of the parent. Example of
`mutator:issuer` and `bubble_on_equip:false` is triggering some kind of condition like an NFT bonus
having expired, or Ragnarok on Viking-themed NFTs in a common collection, etc.

## Conditional

TBD

## Logic

For Logic attributes please see [Logic](logic.md).

## Caveats

### Relationship with Metadata Attributes

On-chain attributes that match an attribute from metadata by name are prioritized. E.g. if an NFT
has the attribute `sport:football` in its metadata, then setting the NFT's mutable attribute `sport`
to `basketball` shows `basketball` as the value henceforth, ignoring the one in metadata.
