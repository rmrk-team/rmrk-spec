# Metadata

## Abstract

This proposal is for NFT metadata schema and standard.

The document is broken down into two main sections: 1) The metadata schema, and 2) recommended properties to use for different types of NFTs.

## Acknowledgments

The standard is loosely based on:

- [Tezos TZIP-21 metadata standard](https://tzip.tezosagora.org/proposal/tzip-21)
- [EIP-1155 metadata standard](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema)
- [Opensea's and ERC-721 metadata standard](https://docs.opensea.io/docs/metadata-standards)

This document's structure is heavily inspired by [Tezos TZIP-21 metadata standard](https://tzip.tezosagora.org/proposal/tzip-21)

## Motivation & Goals

This metadata standard aims to:

1. Simplify the creation of rich metadata for NFTs.
2. Provide a commonly understood interface for clients, renderers and other consumers
3. Provide a recommended set of properties to render [composable NFTs](./base.md) of various media types
4. Be extensible
5. Provide interoperability among ecosystem members (contracts, indexers, wallets, libraries, etc)

## Table of Contents

1. [Standards and Recommendations](#standards-and-recommendations)
2. [Schema Definition](#schema-definition)

## Standards and Recommendations

_All fields are defined and described in the [Schema Definition](#schema-definition) section of the document._

_More examples for recommended use cases can be added with a simple PR._

It is strongly advised -- but not required -- that all NFTs follow the following
standards and recommendations.

Within RMRK standard, following `metadata` standard can be used on the root of NFT as well as on [resources](../entities/nft.md#resources-and-resource)

A `metadata` field can be added to 3 types of entities

- NFT
- NFT resource
- Base part

### Simple Metadata on NFT or a Resource

Metadata referencing an image media file. This can be used on NFT with no [secondary resources](../entities/nft.md#resources-and-resource) or on a simple secondary Resource. 

#### Example:

```json

{
  "name": "Cozy Fireplace",
  "description": "The Christmas spirit backdrop NFT minted on [kanaria](https://kanaria.com)",
  "mediaUri": "ipfs://ipfs/QmbKQaLAyXB8xV8bnnmskVCLPMs36yRso23xWzpwgHyPui/xmas21_cozyfireplace_background.png",
  "thumbnailUri": "ipfs://ipfs/QmbKQaLAyXB8xV8bnnmskVCLPMs36yRso23xWzpwgHyPui/xmas21_cozyfireplace_background_thumb.png",
  "externalUri": "https://kanaria.rmrk.app",
  "license": "BAYC Commercial Use",
  "licenseUri": "https://boredapeyachtclub.com/#/terms",
  "properties": {
    "rarity": {
      "type": "string",
      "value": "legendary"
    }
  }
}

```

### Composable SVG resource's metadata

Metadata on a resource compatible with SVG base part as described in [Base](./base.md#parts)

_Note that resource spec also allows you to add image and thumbnail directly on resource on-chain using `src` and `thumb` fields, but in this case we will use `metadata`_

#### Example:

```json
{
  "name": "A cool hat",
  "description": "A cool hat compatible with a cool bird.",
  "mediaUri": "ipfs://ipfs/xxx/hat.svg",
  "thumbnailUri": "ipfs://ipfs/xxx/hat_thumb.png",
  "externalUri": "https://kanaria.rmrk.app",
  "properties": {
    "rarity": {
      "type": "string",
      "value": "rare"
    },
    "mimeType": {
      "type": "string",
      "value": "image/svg+xml"
    }
  }
}
```

### Composable audio resource's metadata

Metadata on a resource compatible with Audio base part as described in [Base](./base.md#parts)

_Note that resource spec also allows you to add image and thumbnail directly on resource on-chain using `src` and `thumb` fields, but in this case we will use `metadata`_

#### Example:

```json

{
  "name": "Drums 001",
  "description": "Drum stems",
  "mediaUri": "ipfs://ipfs/XXXX.flac",
  "thumbnailUri": "ipfs://ipfs/XXXX.png",
  "externalUri": "https://kanaria.rmrk.app",
  "license": "BAYC Commercial Use",
  "licenseUri": "https://boredapeyachtclub.com/#/terms",
  "properties": {
    "mimeType": {
      "type": "string",
      "value": "audio/flac"
    }
  }
}
```

### SVG Base part of type slot

Metadata on a base part of type slot as described in [Base](./base.md#parts)

_Note that base part's spec also allows you to add `type: "slot"` media file and `z` directly on base part on-chain using `type`, `src` and `z` fields, but in this case we will use `metadata`_

#### Example:

```json

{
  "name": "Kanaria headwear slot",
  "description": "This is a Kanaria headwear slot, you can equip nested NFTs with a compatible resource in it.",
  "mediaUri": "ipfs://ipfs/XXXX/headwear_slot_fallback.svg",
  "type": "slot",
  "properties": {
    "z": {
      "type": "int",
      "value": 2
    }
  }
}
```

### Audio Base part of type slot

Metadata on a base part of type slot as described in [Base](./base.md#parts)

_Note that base part's spec also allows you to add `type: "slot"` media file and `z` directly on base part on-chain using `type`, `src` and `z` fields, but in this case we will use `metadata`_

#### Example:

```json

{
  "type": "slot",
  "properties": {
    "volume": {
      "type": "float",
      "value": 0.2
    },
    "autoplay": {
      "type": "boolean",
      "value": true
    },
    "loop": {
      "type": "boolean",
      "value": true
    },
    "mute": {
      "type": "boolean",
      "value": false
    },
    "z": {
      "type": "int",
      "value": 2
    },
    "mimeType": {
      "type": "string",
      "value": "audio/flac"
    }
  }
}
```

### Reveal mechanic

Because metadata fields of a primary resource of NFT is always takes precedence over metadata on NFT's root this can be used as a reveal mechanic.

Example:

1. Mint NFT with `metadata` field

```json
{
  "id": "nft-id-1",
  "owner": "HeyRMRK7L7APFpBrBqeY62dNhFKVGP4JgwQpcog2VTb3RMU",
  "block": 7777,
  "metadata": "ipfs://ipfs/hash"
}
```

Where the content of the metadata is referencing an EGG image:

```json
{
  "name": "The egg",
  "description": "This egg is going to hatch into something beautiful.",
  "mediaUri": "ipfs://ipfs/XXX/egg.png",
  "thumbnailUri": "ipfs://ipfs/XXX/egg_thumb.png"
}
```

2. Then, once you are ready to hatch the egg for the user, just [Add a resource](../interactions/resadd.md) to this NFT. 

```json
{
  "id": "resource-id-1",
  "metadata": "ipfs://ipfs/hash"
}
```
_Note that resource spec also allows you to add image and thumbnail directly on resource on-chain using `src` and `thumb` fields, but in this case we will use `metadata`_

Where the content of the metadata is referencing a Bird image:

```json
{
  "name": "The bird",
  "description": "This bird has been hatched.",
  "mediaUri": "ipfs://ipfs/XXX/bird.png",
  "thumbnailUri": "ipfs://ipfs/XXX/bird_thumb.png"
}
```

Because the fields of primary resource always take precedence over the same fields of the NFT metadata, the egg will now be the bird as long as the bird resource is the primary one.

## Schema Definition

The schema defines the following additional types:

---

### Base metadata fields

#### `name` (string, required)

NFT name.

#### `description` (string)

General notes, abstracts, or summaries about the contents of an NFT.

#### `type` (string)

A broad definition of the type of content of the NFT.

#### `locale` (string)

Metadata locale in [ISO 639-1](https://www.iso.org/standard/22109.html) format. For translations and localisation. e.g. `en-GB`, `en`, `fr`

#### `license` (string)

A statement about the NFT license.

#### `licenseUri` (string)

A URI to a statement of license.

#### `mediaUri` (string)

A URI to a main media file of the NFT.

#### `thumbnailUri` (string)

A URI to an image of the NFT
for wallets and client applications to have a scaled down image to present to end-users.

Recommend maximum size of 350x350px.

#### `externalUri` (string)

A URI  with additional information about the subject or content of the NFT.

#### `properties` (object)

Custom attributes about the subject or content of the asset.

The object with key-value pairs where key is a unique identifier of an attribute and value is an object of type [Property](#metadata-properties)

All properties key-value pairs are arbitrary except for reserved property [royaltyInfo](#royaltyinfo). The reason why `royaltyInfo` is part of properties is [explained below](#mutable-properties) 

##### Example:

```json
{
  "properties": {
    "rarity": {
      "type": "string",
      "value": "epic"
    },
    "color": {
      "type": "string",
      "value": "red"
    },
    "height": {
      "type": "float",
      "value": 192.4
    },
    "tags": {
      "type": "array",
      "value": ["music", "2020", "best"]
    }
  }
}
```

#### `image` [DEPRECATED]

A URI to a main media file of the NFT.

This is a deprecated field, but some clients can choose to support it for backward compatibility.

#### `external_url` [DEPRECATED]

A URI  with additional information about the subject or content of the NFT. 

This is a deprecated field, but some clients can choose to support it for backward compatibility.

---

## Metadata Properties

All properties key-value pairs are arbitrary except for reserved property [royaltyInfo](#royaltyinfo). The reason why `royaltyInfo` is part of properties is [explained below](#on-chain-properties)

_More recommended properties will be added as we identify common use cases for composable NFTs_

Please note that there are also on-chain Properties fields on NFT. Properties should always be saved in metadata and off-chain to save on storage unless mutable properties are needed, in this case properties can be saved on-chain. See [On-chain properties](#on-chain-properties) for more details.

### Recommended properties

We maintain a list of recommended but optional properties to provide a commonly understood interface for clients, renderers and other consumers.

#### `mimeType` (string)

Media (MIME) type of the format.

See [IANA Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml)

#### `width` (number)

Number value to help clients to size the media file.

#### `height` (number)

Number value to help clients to size the media file.

#### `dimensionUnits` (string)

Units to use for `width` and `height` properties (e.g. `px`)

#### `x` (number)

Number value to position media along the x-axis.

#### `y` (number)

Number value to position media along the y-axis.

#### `z` (number)

Number value to position media along the z-axis.

#### `volume` (number)

The volume of the specific media, between `0.0` to `1.0`.

#### `preferThumb` (boolean)

Tell UIs to display `thumbnailUri` instead of `mediaUri` whenever possible. This is handy if `mediaUri` is of type that doesn't make sense to user out of context i.e. in context of composable NFTs

##### Example:

```json
"volume": {
  "type": "float",
  "value": 0.2
}
```

#### `loop` (boolean)

Set to `true` to let client know that this media should be looped.

#### `autoplay` (boolean)

Set to `true` to let client know that this media should autoplay on render.

#### `mute` (boolean)

Set to `true` to let client know that this media should be muted on render.

#### `offset` (number)

Number value, typically in milliseconds, to offset media. Useful for audio or video media.

#### `duration` (number)

Number value, typically in milliseconds, to set a total duration of the media. Useful for audio or video media.

---

## On-chain Properties

On-chain properties follow the same format as [metadata properties](#metadata-properties) but can be set as mutable. Properties should always be saved in metadata unless mutable properties are needed, in this case properties can be saved on-chain. See [On-chain properties](#on-chain-properties) for more details.

_On-chain properties should always be merged and used over metadata/ipfs properties. Thus `royaltyInfo` on chain would always be used over `royaltyInfo` on metadata_

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

### Recommended properties

At the moment of writing, the only reserved and recommended on-chain property is `royaltyInfo`.

#### `royaltyInfo`

```json
"properties":  {
  "royaltyInfo": {
    "_mutation":  {
      "allowed": true
    },
    "type": "royalty",
    "value": {
      "receiver": "xxx",
      "royaltyPercentFloat": 0.2,
    }
  }
},
```

