# SETPROPERTY interaction (Kusama)

This document describes Kusama-specific examples and caveats for the [SETPROPERTY](../../abstract/interactions/setproperty.md) interaction.  See the [Abstract](../../abstract/interactions/setproperty.md) for full specs.

Format (Kusama):
```txt
rmrk::SETPROPERTY::2.0.0::{id}::{html_encoded_name}::{html_encoded_value}
```

Examples (Kusama):
Given an NFT `5105000-0aff6865bed3a66b-DLEP-DL15-00000001` with a property (either
inherited from Collection or directly defined on NFT):

```json
"properties":  {
  "color": {
    "_mutation":  {
      "allowed": true
    },
    "type": "string",
    "value": "blue"
  }
},
```

We can change the color to red like so:

```txt
RMRK::SETPROPERTY::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::color::red
```
