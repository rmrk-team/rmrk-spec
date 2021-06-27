# Base

A base is a meta-entity, stored on-chain or off chain. It is not an NFT, but can be thought of as
the interface or abstract class for an NFT's render.

A base is a JSON object with three properties: `id`, `type`, and `parts`. There is an implied
`issuer` field, matching the address that created the base.

Example:

```json
{
    "id": "kanaria_superbird",
    "type": "svg",
    "parts": [
      {
          "id": "bg",
          "type": "fixed",
          "z": 0,
          "src": "ipfs://ipfs/hash"
      },
      {
          "id": "gem_1",
          "type": "slot",
          "equippable": ["id-of-genesis-trait-crystals-LEGENDARY"],
          "unequip": "unequip",
          "z": 1
      },
      {
          // ...
      },
      {
          "id": "wing_1_back",
          "type": "fixed",
          "z": 1,
          "src": "ipfs://ipfs/hash"
      },
      {
          "id": "wing_1_front",
          "type": "fixed",
          "z": 3,
          "src": "ipfs://ipfs/hash2"
      },
      {
          "id": "wing_1_slot",
          "type": "slot",
          "equippable": ["id-of-genesis-legendaries", "id-of-genesis-rares", "id-of-genesis-epics", ...],
          "unequip": "burn",
          "z": 2
      }
    ]
}
```

## Base ID

This is an arbitrary value, used for namespacing part slots. See Parts below. It can be any string
value but must not use dashes. `kanaria-epic-birds` is not OK, but `kanaria_epic_birds` is. This is because the computed ID property (see below) uses dashes `-` to combine additiaonl elements adding uniqueness to the ID.

## Base Type

The type is a pre-defined value that clearly specifies how an NFT with a certain base type is
supposed to be rendered. Currently, the only supported value is SVG:

```json
"type": "svg"
```

### SVG Base

The parts will have Z indexes for layered rendering, and the final image is a composited, layered
SVG. The SVG type implies the following rules:

- every part will have a render in the same viewport
- every part will be vector graphics
- every part's 0,0 coordinates are in the top left corner

This makes it possible to overlay SVG assets on top of each other without worrying about location in
the rendering space. Example:

![SVG composition](../images/svg_composition.png)

The image above shows three different SVG assets, absolutely positioned within the same viewport.
Notice how the viewport does not scale to bound the SVG asset, but instead scales to match the full
size of the composited SVG (indicated by the faint bird outline). When combining these SVGs one on
top of the other, the composited effect is achieved. This is possible because of part Z-index
definitions (see below) and SVG transparency.

#### SVG Base Parts

`parts` is an array of Part objects. A Part object has two types: `fixed` and `slot`. Each part has
an ID, uniquely identifying it within this base.

**Fixed** parts are references to static content, like an IPFS hash of an SVG file, and a `z` index
value, like so:

```json
  {
      "id": "wing_1_front",
      "type": "fixed",
      "z": 3,
      "src": "ipfs://ipfs/hash2"
  },
```

The renderer will take the content behind the `src` value, and place it into the viewport at the Z
index defined. The renderer does not check that the viewport of all parts matches - this is up to
the designer (refer to the image above).

**Slot** parts have a type of `slot` and no static resource `src`. They are meant to visually accept
other NFTs into them. They have an array of whitelisted equippable classes, and an optional
unequip value.

```json
{
    "id": "wing_1_slot",
    "type": "slot",
    "equippable": ["id-of-genesis-legendaries", "id-of-genesis-rares", "id-of-genesis-epics", ...],
    "unequip": "burn",
    "z": 2
}
```

The `equippable` value is a list of classes equippable into this slot. This value can also be a
wildcard `*` to allow any class to be equippable into this slot without special approval from
the base issuer. The whitelisting can be useful to prevent others from "hijacking" your project with
their own customizations, covering all your art and branding.

The optional `unequip` value specifies what happens to an item once it's unequipped. This is useful
for one-off bonuses, NFTs that expire, and so on. The values can be `inv` for inventory and `burn`.

### Other Types

Different types are implementable by individual projects and users. For example, a 3D type might
define coordinates in 3D space instead of a Z index. If a renderer proves especially useful and well
defined, we suggest the implementer propose it as a RIP upgrade to this Base standard.

## Full Spec

### Base

```json
{
  "type": {
    "type": "string",
    "description": "Type of base",
    "values": ["svg"]
  },
  "id": {
    "type": "string",
    "description": "Any arbitrary unique string value. Consolidator should throw errors on duplicate IDs."
  },
  "parts": Part[]
}
```

### Computed fields

Computed fields are fields that are used in interactions, but are not explicitly set on their
entities. Computed fields are the result of applying a standardized calculation or merger formula to
specific fields. The Base entity has the following computed fields, to be provided by
implementations:

```json
{
  "issuer": {
    "type": "computed",
    "description": "Chain-specific address of the original creator of the Base, or address of new issuer if issuer was changed with the CHANGEISSUER interaction."
  },
  "id": {
    "type": "computed",
    "description": "A Base is uniquely identified by the combination of the word `base`, its minting block number, and user provided ID during Base creation, glued by dashes `-`, e.g. base-4477293-kanaria_superbird."
  }
}
```

### Parts

```json
{
  "id": {
    "type": "string",
    "description": "Defines unique ID of part in this base"
  },
  "type": {
    "type": "string",
    "description": "Defines fixed or slot part",
    "values": ["fixed", "slot"]
  },
  "equippable": {
    "type": "string[]",
    "description": "A list of Class IDs (see NftClass entity) containing NFTs equippable into slots of this base."
  },
  "unequip": {
    "type": "string",
    "description": "What to do with an NFT that is removed from a slot. `burn` destroys the NFT (see CONSUME for effects), and `inv` just empties the slot.",
    "values": ["burn", "inv"]
  },
  "src": {
    "type": "string",
    "description": "URL to resource, or direct SVG data (<svg></svg>)."
  },
  "z": {
    "type": "number",
    "description": "Only applies to SVG base. Defines layer height of SVG element."
  }
}
```

## Interactions

- [BASE](../interactions/base.md) - creates a base
- [EQUIP](../interactions/equip.md) - changes the equippable class set
- [CHANGEISSUER](../interactions/changeissuer.md) - changes the issuer
