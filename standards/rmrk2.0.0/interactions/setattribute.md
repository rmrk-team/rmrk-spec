# SETATTRIBUTE

The `SETATTRIBUTE` interaction allows NFT owners to change certain mutable values on their NFTs.

The types of on-chain attributes susceptible to this interaction are: mutable, conditional, logic.

## Mutable

Attributes are mutable if they are defined to be mutable by the minter (see
[NFT On-chain Attributes](../entities/base.md#on-chain-attributes)).

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

Some attributes can only be mutated if a condition is reached. See
[list of conditions for mutation](#list-of-mutation-conditions).

Conditional attributes will be defined like this:

```json
{
  "mutable": true,
  "trait_type": "Hair color",
  "value": "blue",
  "mut_condition": {
    "hasfrom": ["0aff6865bed3a66b-HAIRDYE"],
    "constrain": "attributes.color",
    "outcome": "burn"
  }
}
```

In other words, the Hair color attribute can only be changed if this NFT also owns an NFT from the
`0aff6865bed3a66b-HAIRDYE` collection. The value can only be mutated to whatever value the `color`
attribute of the NFT we're using is. So, being unable to color `blue` hair `red` with a `green` dye,
but only with a `red` dye.

Because attributes can be on IPFS, it is recommended to leave consolidation and fetching of
attribute mutations for later, and fetch on-demand, or fetch once and cache heavily with dumps.

When using `hasfrom` or `equippedfrom`, the interaction has the following format:

```txt
rmrk::SETATTRIBUTE::2.0.0::{id}::{html_encoded_name}::{html_encoded_value}::{id_with}
```

- `id_with` is the ID of the item being used for this, and to be validated against the condition

### List of Mutation conditions

#### Has From

```json
  "mut_condition": {
    "hasfrom": ["0aff6865bed3a66b-HAIRDYE"],
    "constrain": "attributes.{attr}" | "none",
    "outcome": "burn" | "inv" | "none"
  }
```

- `hasfrom` defines a list of collections to which the owned NFT that is the condition for this
  change (the object NFT) can belong.
- `constrain` can be `none`, allowing any value, or `attributes.{attr}` where `{attr}` is any
  attribute on the NFT being used to enact this change (`id_with`)
- `outcome` does nothing if `none`, unequips is `inv` and obeys the slot's `unequip` value to
  resolve (e.g. burns if slot's unequip value is `burn`), and [CONSUME](consume.md)s the object NFT
  if set to `burn`, adding the memo `setattribute:{attribute}`.

#### Equipped From

```json
  "mut_condition": {
    "equippedfrom": ["0aff6865bed3a66b-HAIRDYE"],
    "constrain": "attributes.{attr}" | "none",
    "outcome": "burn" | "inv" | "none"
  }
```

Same as above, but the object NFT has to also be equipped.

## Logic

For Logic attributes please see [Logic](logic.md).

## Caveats

### Relationship with Metadata Attributes

On-chain attributes that match an attribute from metadata by name are prioritized. E.g. if an NFT
has the attribute `sport:football` in its metadata, then setting the NFT's mutable attribute `sport`
to `basketball` shows `basketball` as the value henceforth, ignoring the one in metadata.
