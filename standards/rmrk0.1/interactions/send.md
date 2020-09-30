# SEND

Send an [NFT](../entities/nft.md) to an arbitrary recipient.

## Standard

The format of a SEND interaction is `0x{bytes(rmrk::SEND::{version}::{id}::{recipient})}`.

- `version` is the version of the standard used (e.g. `0.1`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entity/nft.md/#computed-fields).
- `recipient` is the addres of the recipient, e.g. `H9eSvWe34vQDJAWckeTHWSqSChRat8bgKHG39GC1fjvEm7y`

## Examples

```
rmrk::SEND::0.1::0aff6865bed3a66b-DLEP-DL15-0000000000000001::H9eSvWe34vQDJAWckeTHWSqSChRat8bgKHG39GC1fjvEm7y
```

Is submitted as:

```
0x726d726b3a3a53454e443a3a302e313a3a306166663638363562656433613636622d444c45502d444c31352d303030303030303030303030303030313a3a4839655376576533347651444a4157636b6554485753715343685261743862674b48473339474331666a76456d3779
```
