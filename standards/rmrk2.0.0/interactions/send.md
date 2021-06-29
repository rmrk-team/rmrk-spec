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

## Examples

```
rmrk::SEND::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001::H9eSvWe34vQDJAWckeTHWSqSChRat8bgKHG39GC1fjvEm7y
```