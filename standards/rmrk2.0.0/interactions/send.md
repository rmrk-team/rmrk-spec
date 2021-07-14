# SEND

Send an [NFT](../entities/nft.md) to an arbitrary recipient.

You can only SEND an existing NFT (one that has not been [CONSUMEd](consume.md) yet).

When sending an NFT to another NFT, unless you own both, the SEND has to be [ACCEPT](accept.md)ed.

## Standard

The format of a SEND interaction is `0x{bytes(rmrk::SEND::{version}::{id}::{recipient})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).
- `recipient` is the address of the recipient, e.g.
  `H9eSvWe34vQDJAWckeTHWSqSChRat8bgKHG39GC1fjvEm7y` or the ID of the recieving NFT, e.g.
  `5105000-0aff6865bed3a66b-DLEP-DL15-00000001`

## Effects

This interactions cancels any pending [LIST](list.md) on the NFT with this ID. It is equivalent to
having called LIST with a cancel on it. It also cancels any pending SENDs when doing NFT to NFT
transfer.

## Example of sending to NFT

It is possible to SEND an NFT directly from an NFT to another NFT, even if the destination NFT is
owned by yet another NFT.

Suppose we have NFT P1 for Parent 1, which owns NFTs P1C1 and P1C2. Suppose we also have P2 which
owns P2C1 and P2C2. We assume that all NFTs are owned by the same account and thus no
[ACCEPT](accept.md) interaction is necessary.

This makes for the following layout of `children`:

P1:

```json
"children": [
  {
    "id": "P1C1",
    "equipped": "",
    "pending": false,
  },
  {
    "id": "P1C2",
    "equipped": "",
    "pending": false,
  },
]
```

P2:

```json
"children": [
  {
    "id": "P2C1",
    "equipped": "",
    "pending": false,
  },
  {
    "id": "P2C2",
    "equipped": "",
    "pending": false,
  },
]
```

Suppose we want to send P2C1 to P1C1, so that P1C1 becomes the owner of P2C1.

We would issue a command like this:

```
rmrk::SEND::2.0.0::P2C1::P1C1
```

In a consolidator, this is what should happen:

- check if issuer of the SEND interaction === root owner of B1P2 and is so...
- get owner of P2C1
- remove P2C1 from `children` array of P2
- add P2C1 to `children` array in P1

The end result is:

P1:

```json
"children": [
  {
    "id": "P1C1",
    "equipped": "",
    "pending": false,
  },
  {
    "id": "P1C2",
    "equipped": "",
    "pending": false,
  },
]
```

P2:

```json
"children": [
  {
    "id": "P2C2",
    "equipped": "",
    "pending": false,
  },
]
```

P1C1:

```json
"children": [
  {
    "id": "P2C1",
    "equipped": "",
    "pending": false,
  },
]
```

Notice how P1 does not care about its "grandchild". It only keeps a registry of immediate children.
Further lookups can then be made on-demand by the NFT's ID into as many levels as necessary. This
prevents stack overflow and nesting complexity, keeping storage requirements low and maintaining a
2-write-ops maximum per SEND (remove child, add child), not counting any canceled LISTs and other
implied interactions.

## Example of sending to account

```
rmrk::SEND::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::H9eSvWe34vQDJAWckeTHWSqSChRat8bgKHG39GC1fjvEm7y
```
