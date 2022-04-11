# BASE

The BASE interaction creates a [Base](../entities/base.md).

## Standard

The format of a BASE interaction is `0x{bytes(rmrk::BASE::{version}::{html_encoded_json})}`. In
other words, HTML encode the minified JSON, then convert the entire remark with the interaction
prefix into bytes and send with a `0x` prefix.

Refer to the [Base](../entities/base.md) spec for an example of the Base JSON.
