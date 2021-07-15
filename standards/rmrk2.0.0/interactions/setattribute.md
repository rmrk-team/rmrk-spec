# SETATTRIBUTE

The `SETATTRIBUTE` interaction allows NFT owners to change certain mutable values on their NFTs.

The types of on-chain attributes susceptible to this interaction are: mutable, conditional, logic.

## Mutable

Attributes are mutable if they are defined to be mutable by the minter (see
[NFT On-chain Attributes](../entities/base.md#on-chain-attributes)), or if they belong to the
[list of always-mutable attributes](#list-of-always-mutable-attributes).

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

## Conditional

TBD

## Logic

For Logic attributes please see [Logic](logic.md).

## Caveats

### Relationship with Metadata Attributes

On-chain attributes that match an attribute from metadata by name are prioritized. E.g. if an NFT
has the attribute `sport:football` in its metadata, then setting the NFT's mutable attribute `sport`
to `basketball` shows `basketball` as the value henceforth, ignoring the one in metadata.

### List of Always Mutable Attributes

| Attribute  | Description                                                                 | Values                        |
| ---------- | --------------------------------------------------------------------------- | ----------------------------- |
| `priority` | Defines rendering and usage order of various resources on an individual NFT | Array of ordered resource IDs |
