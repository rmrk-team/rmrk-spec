# SEND

Send an [NFT](../entities/nft.md) to an arbitrary recipient.

You can only SEND an existing NFT (one that has not been [CONSUMEd](consume.md) yet).

## Standard

The format of a SEND interaction is `0x{bytes(rmrk::SEND::{version}::{id}::{recipient})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).
- `recipient` is the address of the recipient, e.g.
  `H9eSvWe34vQDJAWckeTHWSqSChRat8bgKHG39GC1fjvEm7y`

## Effects

This interactions cancels any pending [LIST](list.md) on the NFT with this ID. It is equivalent to
having called LIST with a cancel on it.

It is possible to SEND an NFT directly from an NFT to another NFT, even if the destination NFT is owned by yet another NFT.

Suppose we have NFT P1 for Parent 1, which owns NFTs B1P1 and B2P1. Suppose we also have P2 which owns B1P2 and B2P2.

This makes for the following layout of `children`:

P1:

```
"children": [
    {"B1P1": ""},
    {"B2P1": ""},
]
```

P2:

```
"children": [
    {"B1P2": ""},
    {"B2P2": ""},
]
```

Suppose we want to send B1P2 to B1P1, so that B1P1 becomes the owner of B1P2.

We would issue a command like this:

```
rmrk::SEND::2.0.0::B1P2::B1P1
```

In a consolidator, this is what should happen:

- check if issuer of the SEND interaction === root owner of B1P2 and is so...
- get owner of B1P2
- remove B1P2 from `children` array of P2
- add B1P2 to `children` array in P1

The end result is:

P1:

```
"children": [
    {"B1P1": ""},
    {"B2P1": ""},
]
```

P2:

```
"children": [
    {"B2P2": ""},
]
```

B1P1:

```
"children": [
    {"B1P2": ""}
]
```

Notice how P1 does not care about its "grandchild". It only keeps a registry of immediate children. Further lookups can then be made on-demand by the NFT's ID into as many levels as necessary. This prevents stack overflow and nesting complexity, keeping storage requirements low and maintaining a 2-write-ops maximum per SEND (remove child, add child), not counting any canceled LISTs and other implied interactions.

## Examples

```
rmrk::SEND::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001::H9eSvWe34vQDJAWckeTHWSqSChRat8bgKHG39GC1fjvEm7y
```