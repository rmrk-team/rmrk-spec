# SETPRIORITY interaction (Kusama)

This document describes Kusama-specific examples and caveats for the [SETPRIORITY](../../abstract/interactions/setpriority.md) interaction.  See the [Abstract](../../abstract/interactions/setpriority.md) for full specs.

## Format (Kusama)

```txt
rmrk::SETPRIORITY::2.0.0::{id}::{html_encoded_value}
```

- `html_encoded_value` is the full array of resource IDs. It is **always** a comma-separated string
  of IDs.

## Examples (Kusama)

Given an NFT `5105000-0aff6865bed3a66b-DLEP-DL15-00000001` with resources `foo`, `bar`, and
`baz`, if we want to make `bar` the priority resource, and `baz` the least important, we issue the
following:

```txt
rmrk::SETPRIORITY::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::bar,foo,baz
```
