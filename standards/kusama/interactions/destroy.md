# DESTROY interaction (Kusama)

This document describes Kusama-specific examples and caveats for the [DESTROY](../../abstract/interactions/destroy.md) interaction.  See the [Abstract](../../abstract/interactions/destroy.md) for full specs.

## Standard (Kusama)

The format of DESTROY interaction is: `0x{bytes(rmrk::DESTROY::{version}::{id})}`

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [collection](../entities/collection.md)'s ID.

## Example (Kusama)

`RMRK::DESTROY::2.0.0::0a24c7876a892acb79-PUNKS`
