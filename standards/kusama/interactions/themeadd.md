# THEMEADD interaction (Kusama)

This document describes Kusama-specific examples and caveats for the [THEMEADD](../../abstract/interactions/themeadd.md) interaction.  See the [Abstract](../../abstract/interactions/themeadd.md) for full specs.

## Format (Kusama)
```
0x{bytes(rmrk::THEMEADD::{version}::{base_id}::{name}::{html_encoded_json})}.
```
- `version` is the version of the standard used (e.g. `2.0.0`)
- `base_id` is the [base](../entity/base.md)'s ID.
- `name` is the theme name
- `html_encoded_json` is the html encoded data, formatted according to the [THEMEADD](../../abstract/interactions/themeadd.md) standards