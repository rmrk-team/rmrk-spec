# Base

A base is a meta-entity, a catalogue of parts. It is not an [NFT](nft.md), but in developer-terms
can be thought of as the interface or abstract class of an NFT's render i.e. final user-facing
output.

A base is a JSON object with following properties: `symbol`, `type`, `themes` and `parts`. There is an implied
`issuer` field, matching the address that created the base, and an `id` field which is dynamically
generated based on the `symbol` and some other values (see Computed Properties below).

Example:

```json
{
    "symbol": "kanaria_superbird",
    "type": "svg",
    "parts": [
      {
          "id": "bg",
          "src": "ipfs://ipfs/hash",
          "type": "slot",
          "equippable": ["collection_1", "collection_2"],
          "z": 3
      },
      {
          "id": "gem_1",
          "src": "ipfs://ipfs/hash",
          "type": "fixed",
          "z": 4
      },
      {
          "id": "wing_1_back",
          "src": "ipfs://ipfs/hash",
          "metadata": "ipfs://ipfs/hash"
      },
      {
          "id": "wing_1_front",
          "metadata": "ipfs://ipfs/hash2"
      }
    ]
}
```

#### symbol (string)

An arbitrary value used for namespacing part slots (see Parts below). It can be any string value,
but must not use dashes or dots. `kanaria-epic-birds` is not OK, but `kanaria_epic_birds` is.

This is because the computed `id` property uses dashes `-` to combine additional elements, adding
uniqueness to the ID, and because dots `.` are used for `slot` addressing.

#### type (string)

An optional and arbitrary type to help clients with rendering instructions. We recommend following types `svg`, `audio`, `mixed`, `video`, `png`

## Full Spec

### Base

####  `type` (string)

An optional and arbitrary type to help clients with rendering instructions. We recommend following types `svg`, `audio`, `mixed`, `video`, `png`

#### `metadata` (string)

An optional ipfs Uri pointing to a [Metadata](./metadata.md) JSON.

#### `parts` (array)

An array of ALL the possible [base parts](#base-parts) an NFT inheriting this base can be rendered with.

#### `themes`

A mutable theme list to cherry-pick from when this Base is added to a NFT. See [Themes](#themes)

### Computed Fields

Fields used in interactions but not explicitly set on their entities. Instead, they are calculated
from previous applied interactions.


#### `issuer` (string Address)

Chain-specific address of the original creator of the Base, or address of new issuer if issuer was changed with the CHANGEISSUER interaction.

#### `id` (string)

A unique identifier.

In extrinsic RMRK implementation, a Base is uniquely identified by the combination of the word `base`, its minting block number, and user provided symbol during Base creation, glued by dashes `-`, e.g. base-4477293-kanaria_superbird.

---

## Base Parts

`parts` is an array of Part objects. Each part has an ID, uniquely identifying it within the base.

> `parts` is a full catalogue of Parts which an NFT using this base as a resource can cherry-pick
> from in order to achieve a composite render.

### Parts schema definition

#### `src` (string)

An optional IPFS Uri pointing to main media file of this part

#### `type` (string)

A Base part can be of type `fixed` or `slot`. A compatible resource can be equipped in a `slot` part. 

#### `z` (number)

`z` is an optional zIndex of base part layer. It is used to help clients and renderers to display composable NFT.

#### `equippable` (array)

Array of whitelisted collections that can be equipped in the given slot. Used with `slot` parts only.

#### `metadata` (string)

An optional ipfs Uri pointing to a [Metadata](./metadata.md) JSON.

---

## Metadata

Some fields on base parts can be saved off-chain using `metadata` field to save up on on-chain storage. The on-chain fields should always be used if present regardless of metadata content.

- If a base part has `metadata` field but no `src` field, then [mediaUri](./metadata.md#mediauri-string) field from metadata is used instead as a main media file of that part.
- If a base part has `metadata` field but no `z` field, then [z](./metadata.md#z-number) field from the properties of metadata JSON is used instead.
- If a base part has `metadata` field but no `type` field, then [type](./metadata.md#type-string) field from the metadata JSON is used instead. It is strongly recommended keeping `type` on part rather than on metadata to aid with querying.


## Examples

### Using on-chain fields

```json
{
  "symbol": "kanaria_superbird",
  "parts": [
    {
      "id": "bg",
      "src": "ipfs://ipfs/hash",
      "type": "slot",
      "equippable": ["collection_1", "collection_2"],
      "z": 3
    },
    ...
  ]
}
```


### Using off-chain fields from metadata

```json
{
  "symbol": "kanaria_superbird",
  "parts": [
    {
      "id": "bg",
      "metadata": "ipfs://ipfs/hash"
    },
    ...
  ]
}
```

And the content of metadata would be 

```json
{
  "mediaUri": "ipfs://ipfs/QmZy8eRLhToqPk5154SJkTJfPD8AMnPAjBi6w1S61yNPrR/1F32B/1f32b_eyes.svg",
  "thumbnailUri": "ipfs://ipfs/QmZy8eRLhToqPk5154SJkTJfPD8AMnPAjBi6w1S61yNPrR/1F32B/1f32b_eyes_thumb.png",
  "type": "slot",
  "properties": {
    "mimeType": {
      "type": "string",
      "value": "image/svg+xml"
    },
    "z": {
      "type": "int",
      "value": 2
    },
    "zName": {
      "type": "string",
      "value": "Hats"
    },
  }
}
```
---

## SVG composition example

![SVG  composition](../images/svg_composition.png)

The image above shows three different SVG assets, absolutely positioned within the same viewport.
Notice how the viewport does not scale to bound the SVG asset, but instead scales to match the full
size on the composited SVG (indicate by the faint bird outline). When placing these SVGs one on top
of the other, the composited effect is achieved. This is possible because of Z-indexing (see below)
and SVG transparency.

> `parts` is a full catalogue of Parts which an NFT using this base as a resource can cherry-pick
> from in order to achieve a composite render.

`fixed` parts are references to static content, like an IPFS hash of an SVG file, and a `z` index
value:

```json
{
  "id": "wing_1_front",
  "type": "fixed",
  "themable": true,
  "z": 3,
  "src": "ipfs://ipfs/hash2"
},
```

`slot` parts have the same content, but `src` (or `mediaUri` in case of metadata) field is optional and used as a fallback for when nothing is equipped in the slot.

### Other types

Different Base type examples can be implemented by other users, and will be made canonical through a
[RIP](https://github.com/rmrk-team/rmrk-spec#contributing) if implemented well. For example, a
composited 3D NFT is not out of the question, but would need coordinates in 3D space, possibly
rigging, and other complexities. If there are any fields that a 3d Base type needs than these can be added to a spec, so consumers can build the UI to render their composable NFTs consistently.

### Themes (object)

A mutable theme list to cherry-pick from when the Base is added to a NFT. See [Themes](#themes)

A theme name must be unique in a base, and a theme with a key other than `default` CANNOT be added
before there is a theme with the `default` key.

Themes is an objects of key-value pairs where key is a named identifier of a theme and value can either be on-chain object representing the theme key-values or IPFS hash with theme's key values.

#### On chain

```json
{
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

#### IPFS

```json
{
  "themes": {
    "default": "ipfs://ipfs/theme1hash",
    "sepia": "ipfs://ipfs/theme2hash"
  }
}
```

In either case, on-chain or IPFS, a theme is cherry-picked with base resource using a [RESADD](../interactions/resadd.md) by it's `id`

It's a clients/renderers job to then get full `theme` values from the Base entity by theme id on the resource. Once the values are available they can be interpolated into the base's
 parts according to its type. E.g. on an SVG-type base, a `green` theme which defines the
`primary_color` as `0x00ff00` will make sure that every `{primary_color}` placeholder inside of all
 SVG parts of this base (fixed or slots) during rendering get replaced by the `0x00ff00`
value, thus theming them.

Themes are added on creation or later with [THEMEADD](../interactions/themeadd.md).

## Interactions

- [BASE](../interactions/base.md) - creates a base
- [EQUIPPABLE](../interactions/equippable.md) - changes the equippable collection set
- [CHANGEISSUER](../interactions/changeissuer.md) - changes the issuer
