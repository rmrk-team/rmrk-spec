# SETPROPERTY

Discussing here: https://app.clickup.com/6615036/docs/69vzw-1501/69vzw-121

The `SETPROPERTY` interaction allows NFT owners to change certain mutable values on their NFTs.

The types of on-chain properties susceptible to this interaction are: mutable, conditional, logic.

## Mutable

Properties are mutable if they are defined to be mutable by the minter (see
[NFT On-chain Properties](../entities/nft.md#on-chain-properties)).

A mutable property is changed with a SETPROPERTY call:

```txt
rmrk::SETPROPERTY::2.0.0::{id}::{html_encoded_name}::{html_encoded_value}
```

Example. Given an NFT `5105000-0aff6865bed3a66b-DLEP-DL15-00000001` with a property (either
inherited from Collection or directly defined on NFT):

```json
"properties":  {
  "color": {
    "_mutation":  {
      "allowed": true
    },
    "type": "string",
    "value": "blue"
  }
},
```

We can change the color to red like so:

```txt
RMRK::SETPROPERTY::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::color::red
```

If the property was inherited, it appears as an NFT-level property on the NFT henceforth, since
the new value diverges from what was inherited from the Collection.

The property can also have the mutation rule defined with `with` property, which tells consolidator to validate this mutation only if it is paired with specified OP_TYPE 

```json
"properties":  {
  "nickname": {
    "_mutation":  {
      "allowed": true,
      "with": {
        "condition": "d43593c715a56da27d-KANARIAGEMS",
        "opType": "BURN"
      }
    },
    "type": "string",
    "value": "foo"
  }
},
```
Here we are saying that the property `nickname` can only be mutated if `SETPROPERTY` is paired with `BURN` in the same batch call, and `BURN` remark matches provided condition (condition validation is up to a consolidator)

## Logic

For Logic properties please see [Logic](logic.md).

## Caveats

### Relationship with Metadata Properties

On-chain properties that match a property from metadata by name are prioritized. E.g. if an NFT
has the property `sport:football` in its metadata, then setting the NFT's mutable property `sport`
to `basketball` shows `basketball` as the value henceforth, ignoring the one in metadata.

## Standards Per Implementation

[Kusama SETPROPERTY](../../kusama/interactions/setproperty.md)

[Substrate SETPROPERTY](../../substrate/interactions/setproperty.md)

[EVM SETPROPERTY](../../evm/interactions/setproperty.md)