# BASE interaction (Kusama)

This document describes Kusama-specific examples and caveats for the [BASE](../../abstract/interactions/base.md) interaction.  See the [Abstract](../../abstract/interactions/base.md) for full specs.

## Caveats (Kusama)
TODO: caveats for Kusama BASE interaction (or delete if none)

## Standard (Kusama)

The format of a BASE interaction is `0x{bytes(rmrk::BASE::{version}::{html_encoded_json})}`. In
other words, HTML encode the minified JSON, then convert the entire remark with the interaction
prefix into bytes and send with a `0x` prefix.

TODO: Creating a base example (Kusama) and delete the "refer to..." comment

Refer to the [Base](../entities/base.md) spec for an example of the Base JSON.
