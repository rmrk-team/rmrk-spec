# EMOTE interaction (Substrate)

This document describes Substrate-specific examples and caveats for the [EMOTE](../../abstract/interactions/emote.md) interaction.  See the [Abstract](../../abstract/interactions/emote.md) for full specs.

## Standard (Kusama)

The format of a EMOTE interaction is
`0x{bytes(RMRK::EMOTE::{version}::{namespace}::{id}::{emotion})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `namespace` is the namespace to which the ID belongs.
- `id` is the ID of the entity. See namespace table below.
- `emotion` is the unicode of the emoji (e.g Unicode for party popper 🎉 is
  [1F389](https://emojipedia.org/emoji/🎉/).

## Examples (Kusama)

### Emoting on an NFT

```
RMRK::EMOTE::2.0.0::RMRK1::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::1F389
```

### Emoting on an Account

```
RMRK::EMOTE::2.0.0::PUBKEY::0xe81f67c2def10f4cc1f43b0e207921210ff83747eb354ad653bbd2c0f0466f10::1F389
```
