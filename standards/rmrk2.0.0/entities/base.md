# Base

A base is a meta-entity, a catalogue of parts. It is not an [NFT](nft.md), but in developer-terms
can be thought of as the interface or abstract class of an NFT's render i.e. final user-facing
output.

A base is a JSON object with three properties: `symbol` , `type` , and `parts` . There is an implied
`issuer` field, matching the address that created the base, and an `id` field which is dynamically
generated based on the `symbol` and some other values (see Computed Properties below).

Example:

```json
{
    "symbol": "kanaria_superbird",
    "type": "svg",
    "parts": [
      {
          "id": "bg",
          "type": "fixed",
          "z": 0,
          "src": "ipfs://ipfs/hash"
      },
      {
          "id": "gem_1",
          "type": "slot",
          "equippable": ["id-of-genesis-trait-crystals-LEGENDARY"],
          "src": "ipfs://ipfs/default-art-hash",
          "z": 1
      },
      {
          // ...
      },
      {
          "id": "wing_1_back",
          "type": "fixed",
          "z": 1,
          "src": "ipfs://ipfs/hash"
      },
      {
          "id": "wing_1_front",
          "type": "fixed",
          "z": 3,
          "src": "ipfs://ipfs/hash2"
      },
      {
          "id": "wing_1_slot",
          "type": "slot",
          "equippable": ["id-of-genesis-legendaries", "id-of-genesis-rares", "id-of-genesis-epics", ...],
          "z": 2
      }
    ]
}
```

## Base Symbol

An arbitrary value used for namespacing part slots (see Parts below). It can be any string value,
but must not use dashes or dots. `kanaria-epic-birds` is not OK, but `kanaria_epic_birds` is.

This is because the computed `id` property uses dashes `-` to combine additional elements, adding
uniqueness to the ID, and because dots `.` are used for `slot` addressing.

## Base Type

The type is a pre-defined value that specifies how an NFT should be rendered. Currently only `svg`
is supported.

```json
"type": "svg"
```

### SVG Base

The parts will have Z indexes for layered rendering, and the final image is a composited, layered
SVG. The SVG type implies the following:

- every part will have a render in the same viewport at coordinates 0,0
- every part will be SVG

This makes it possible to overlay SVG assets on top of each other without worrying about their
location in the rendering space. For example:

![SVG composition](../images/svg_composition.png)

The image above shows three different SVG assets, absolutely positioned within the same viewport.
Notice how the viewport does not scale to bound the SVG asset, but instead scales to match the full
size on the composited SVG (indicate by the faint bird outline). When placing these SVGs one on top
of the other, the composited effect is achieved. This is possible because of Z-indexing (see below)
and SVG transparency.

#### SVG Base Parts

`parts` is an array of Part objects. A Part object has two types: `fixed` and `slot` and each part
has an ID, uniquely identifying it within the base.

> `parts` is a full catalogue of Parts which an NFT using this base as a resource can cherry-pick
> from in order to achieve a composite render.

`fixed` parts are references to static content, like an IPFS hash of an SVG file, and a `z` index
value:

```json
  {
      "id": "wing_1_front",
      "type": "fixed",
      "themable": true,
      "z": 3,
      "src": "ipfs://ipfs/hash2"
  },
```

The renderer will take the content behind the `src` value and place it into the viewport at the `z`
index defined. The renderer does not check that the viewport matches all parts - this is up to the
designer (refer to the above image).

`themable` means the SVG code contains some placeholder values in the following format:
`{placeholder}`. These get replaced during rendering with static values from a **theme** - see
[THEMEADD](../interactions/themeadd.md).

`slot` parts have a type of `slot` and an optional static resource `src`. They are meant to visually
accept the resources of other NFTs into them. They have an array of whitelisted `equippable`
collections.

If there is a `src` value, this static resource is used as a default fallback art for when the slot
is unequipped. As an example, imagine a playing card with a changeable background - when this slot
is empty, i.e. no custom background is equipped, a default background should be shown.

```json
{
    "id": "wing_1_slot",
    "type": "slot",
    "src": "my-custom-fallback",
    "equippable": ["id-of-genesis-legendaries", "id-of-genesis-rares", "id-of-genesis-epics", ...],
    "z": 2
}
```

The `equippable` value is a list of collections which contain NFTs that have resources compatible
with this slot. This value can also be a wildcard `*` to allow any collection, and it can be `-` to
allow nothing (see [Equippable](../interactions/equippable.md)). The whitelisting can be useful to
prevent others from hijacking your project with their customizations, covering all your art an
branding.

When an NFT inherits a base in its `resources` array, it cherry-picks the `parts` of the base it
needs, like so:

```json
    "resources": [
      {
          "id": "V1StG",
          "base": "some-base-id",
          "parts": ["left_wing_front", "left_wing_back", "gem_slot_1"]
      },
      {
          "id": "Z5i6B",
          "src": "hash-of-guest-bird-art-file",
          "metadata": "hash-of-metadata-with-credits"
      }
    ]
```

This allows us to compose an almost infinite variety of NFTs from a single base's catalogue of
composable parts.

### Other types

Different types can be implemented by other users, and will be made canonical through a
[RIP](https://github.com/rmrk-team/rmrk-spec#contributing) if implemented well. For example, a
composited 3D NFT is not out of the question, but would need coordinates in 3D space, possibly
rigging, and other complexities.

## Full Spec

### Base

```json
{
  "type": {
    "type": "string",
    "description": "Type of base",
    "values": ["svg"]
  },
  "id": {
    "type": "string",
    "description": "Any arbitrary unique string value. Consolidator should throw errors on duplicate IDs."
  },
  "parts": {
    "type": Part[],
    "description": "An array of ALL the possible parts an NFT inheriting this base can be rendered with"
  },
  "themes": {
    "type": {key[string]: Object[k:v]},
    "description": "An object of keyed (named) objects with key value pairs of data that gets interpolated into Parts marked `themable`. See THEMEADD interaction for more information."
  }
}
```

### Computed Fields

Fields used in interactions but not explicitly set on their entities. Instead, they are calculated
from previous applied interactions.

```json
{
  "issuer": {
    "type": "computed",
    "description": "Chain-specific address of the original creator of the Base, or address of new issuer if issuer was changed with the CHANGEISSUER interaction."
  },
  "id": {
    "type": "computed",
    "description": "A Base is uniquely identified by the combination of the word `base`, its minting block number, and user provided symbol during Base creation, glued by dashes `-`, e.g. base-4477293-kanaria_superbird."
  }
}
```

### Parts

The parts array MUST contain ALL the parts that ANY instance of an NFT which uses this base as a
resource could possibly use to be rendered. The NFT using this Base as a resource then cherry-picks
the parts it needs (see [NFT](./nft.md) under Resources).

```json
{
  "id": {
    "type": "string",
    "description": "Defines unique ID of part in this base. Must be alphanumeric, no dots or dashes."
  },
  "type": {
    "type": "string",
    "description": "Defines fixed or slot part",
    "values": ["fixed", "slot"]
  },
  "equippable": {
    "type": "string[]",
    "description": "A list of Collection IDs (see Collection entity) containing NFTs equippable into slots of this base. Starter (default) value is an array with a single, empty string."
  },
  "src": {
    "type": "string",
    "description": "URL to resource, or direct SVG data (<svg></svg>). Optional when type is `slot`."
  },
  "z": {
    "type": "number",
    "description": "Only applies to SVG base. Defines layer height of SVG element."
  }
}
```

### Themes

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
      "secondary_color": "darkyellow"
    },
    "sepia": {
      "primary_color": "0x47822a",
      "secondary_color": "0x331213"
    }
  }
}
```

Themes are added on creation or later with [THEMEADD](../interactions/themeadd.md).

## Interactions

- [BASE](../interactions/base.md) - creates a base
- [EQUIPPABLE](../interactions/equippable.md) - changes the equippable collection set
- [CHANGEISSUER](../interactions/changeissuer.md) - changes the issuer
