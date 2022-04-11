# THEMEADD

The THEMEADD interaction allows the `issuer` of a [base](../entities/base.md) to add a new theme to
the base.

Themes are **named** objects of `variable` => `value` pairs which get interpolated into the base's
`themable` parts according to its type. E.g. on an SVG-type base, a `green` theme which defines the
`primary_color` as `0x00ff00` will make sure that every `{primary_color}` placeholder inside of all
`themable` SVG parts of this base (fixed or slots) during rendering get replaced by the `0x00ff00`
value, thus theming them.

A theme name must be unique in a base, and a theme with a key other than `default` CANNOT be added
before there is a theme with the `default` key.

```json
{
  "type": "svg",
  "parts": [
    {
      "id": "wing_1_front",
      "type": "fixed",
      "z": 3,
      "src": "ipfs://ipfs/hash2",
      "themable": true,
      "..."
    }
  ],
  "themes": {
    "default": {
      "primary_color": "yellow",
      "secondary_color": "darkyellow",
      "_inherit": true
    },
    "sepia": {
      "primary_color": "0x47822a",
      "secondary_color": "0x331213",
      "_inherit": true
    }
  }
}
```

A theme can have any number and type of fields. It is not limited to 2 values, and they do not have
to be colors. A theme such as the following one is perfectly valid:

```json
{
  ...
  "themes": {
    "doglover": {
      "loves": "dogs",
      "favorite_color": "#884023",
      "favorite_breed_logo": "ipfs://ipfs/Qmc982y4r8fh873h928u092jf98h3f98/beagle.svg"
    }
    ...
  }
}
```

A themed SVG looks like this:

```svg
<svg height="100" width="100">
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" data-primary_color_value="red" data-primary_color_attr="fill" fill="red" />
</svg>
```

Notice the data attributes:

- `data-primary_color_attr="fill"`
- `data-primary_color_value="red"`

This means "replace the fill attribute's value with red".

### Theme Inheritance

When an NFT A with theme X is equipped in an NFT B with theme Y, theme Y can inherit theme X values
if its `_inherit` flag is set to `true`.

Parent theme X:

```json
"parent_theme": {
    "primary_color": "blue",
    "dog": "beagle",
    "food": "corn",
    "toy": "ball"
}
```

Child theme Y:

```json
"child_theme": {
    "primary_color": "yellow",
    "number_of_wheels": 4,
    "window_tint": "red",
    "_inherit": true
}
```

Resulting theme applied to child NFT's SVG:

```json
{
  "primary_color": "blue",
  "dog": "beagle",
  "food": "corn",
  "toy": "ball",
  "number_of_wheels": 4,
  "window_tint": "red"
}
```

The result is a merge of the themes, with parent values taking priority over child values of the
same name. If `_inherit` were `false`, the child theme would remain intact.

Non-slot parts defined as `themable: true` are themable and automatically inherit properties from
the parent theme.

> ⚠ There are **two** reserved fields: `_bubble` and `_inherit`. Only `_inherit` is currently used,
> but neither may be set by the user.

## Example

Assume we have a lightsaber which can grow red, green, white, or blue. One could mint four separate
ones, or one could mint a single one and apply the "red", "green", "blue", or "white" theme to each.

The advantages of the latter approach are:

- fewer assets to pin on IPFS and any decentralized storage
- easy to add new looks to an existing collection later on
- less load lag (fewer different resources to load - load one weapon and you loaded them all).

Now, if this lightsaber were to be equipped by a character NFT, it might want to change its
appearance.

Let's assume every lightsaber glows white if not being held of if held by anyone other than Darth
Maul or Obi Wan.

```json
// Theme definition in lightsaber Base
"themes": [
        "default": {
            "primary_color": "#ffffff",
            "_inherit": true
        }
    ]
```

```json
{
  // Obi Wan NFT's resource
    "resources": [
        "id": "VykgH",
        "src": "hash-of-svg-on-ipfs",
        "theme": "default" // <-- this can also be omitted,, which makes the resource use its SVG's built-in fallbacks
    ],
}
```

Now let's assume Obi Wan makes it glow green, and Darth Maul makes it glow red.

```json
// Force user Base themes
"themes": [
        "default": {
          "primary_color": "#ffffff",
        },
        "good": {
          "primary_color": "#00ff00"
        },
        "bad": {
          "primary_color": "#00ff00"
        }
    ]
```

In the NFT itself:

```json
{
  "symbol": "OBIWAN",
  "theme": "good"
}
```

```json
{
  "symbol": "DARTHMAUL",
  "theme": "bad"
}
```

## Format

The format of a `THEMEADD` interaction is:

```
0x{bytes(rmrk::THEMEADD::{version}::{base_id}::{name}::{html_encoded_json})}.
```
