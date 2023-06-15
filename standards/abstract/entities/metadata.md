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

Within RMRK standard, following `metadata` standard can be used on the root of NFT as well as on [assets](../entities/nft.md#resources-and-resource)

A `metadata` field can be added to 3 types of entities

- NFT
- NFT asset
- Catalog part

### Simple Metadata on NFT or a Asset

Metadata referencing an image media file. This can be used on NFT with no [secondary assets](../entities/nft.md#resources-and-resource) or on a simple secondary Asset. 

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
  "attributes": [
    {
      "label": "rarity",
      "type": "string",
      "value": "legendary",
      // For Opensea backward compatibility
      "trait_type": "rarity"
    }
  ]
}

```

### Composable SVG asset's metadata

Metadata on a asset compatible with SVG base part as described in [Catalog](./base.md#parts)

_Note that asset spec also allows you to add image and thumbnail directly on asset on-chain using `src` and `thumb` fields, but in this case we will use `metadata`_

#### Example:

```json
{
  "name": "A cool hat",
  "description": "A cool hat compatible with a cool bird.",
  "mediaUri": "ipfs://ipfs/xxx/hat.svg",
  "thumbnailUri": "ipfs://ipfs/xxx/hat_thumb.png",
  "externalUri": "https://kanaria.rmrk.app",
  "attributes": [
    {
      "label": "rarity",
      "type": "string",
      "value": "rare",
      // For Opensea backward compatibility
      "trait_type": "rarity"
    }
  ]
}
```

### SVG Catalog part of type slot

Metadata on a base part of type slot as described in [Catalog](./base.md#parts)

_Note that base part's spec also allows you to add `type: "slot"` media file and `z` directly on base part on-chain using `type`, `src` and `z` fields, but in this case we will use `metadata`_

#### Example:

```json

{
  "name": "Kanaria headwear slot",
  "description": "This is a Kanaria headwear slot, you can equip nested NFTs with a compatible asset in it.",
  "mediaUri": "ipfs://ipfs/XXXX/headwear_slot_fallback.svg",
  "attributes": [
    {
      "label": "rarity",
      "type": "string",
      "value": "rare",
      // For Opensea backward compatibility
      "trait_type": "rarity"
    }
  ]
}
```

### Reveal mechanic

Because metadata fields of a primary asset of NFT is always takes precedence over metadata on NFT's root this can be used as a reveal mechanic.

Example:

1. Mint NFT with `metadata` field on primary asset

Where the content of the metadata is referencing an EGG image:

```json
{
  "name": "The egg",
  "description": "This egg is going to hatch into something beautiful.",
  "mediaUri": "ipfs://ipfs/XXX/egg.png",
  "thumbnailUri": "ipfs://ipfs/XXX/egg_thumb.png"
}
```

2. Then, once you are ready to hatch the egg for the user, just [Replace an asset](../interactions/resadd.md) on this NFT with new metadata reference.

Where the content of the metadata is referencing a Bird image:

```json
{
  "name": "The bird",
  "description": "This bird has been hatched.",
  "mediaUri": "ipfs://ipfs/XXX/bird.png",
  "thumbnailUri": "ipfs://ipfs/XXX/bird_thumb.png"
}
```

## Schema Definition

The schema defines the following additional types:

---

### Base metadata fields

#### `name` (string, required)

NFT name.

#### `description` (string)

General notes, abstracts, or summaries about the contents of an NFT.

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

#### `tags` (array)

An array of string values, used to help marketplaces to categorize your NFT

```json
{
  "tags": ["music", "2020", "best"]
}
```

#### `preferThumb` (boolean)

This flag will signal to UIs to prefer `thumbnailUri` instead of `mediaUri` wherever applicable.

#### `attributes` (array)

Custom attributes about the subject or content of the asset.

An array of attribute objects

##### Example:

```json
{
  "attributes": [
    {
      "label": "rarity",
      "type": "string",
      "value": "epic",
      // For Opensea backward compatibility
      "trait_type": "rarity"
    },
    {
      "label": "color",
      "type": "string",
      "value": "red",
      // For Opensea backward compatibility
      "trait_type": "color"
    },
    {
      "label": "height",
      "type": "float",
      "value": 192.4,
      // For Opensea backward compatibility
      "trait_type": "height",
      "display_type": "number"
    }
  ]
}
```

A list of values that Singular marketplace will support for a `type` field are `number`, `float`, `integer`, `string`, `date`, `percentage`, however these are arbitrary and other UIs can implement their own types. 

### Opensea metadata compatiblity

Because RMRK metadata standard is not 100% compatible with Opensea metadata standard, you might want to add Opensea metadata fields alongside RMRK metadata fields for backward compatibility with existing NFT marketplaces. 

#### `image` For backward compatibility with Opensea metadata spec

A URI to a main media file of the NFT.

#### `animation_url` For backward compatibility with Opensea metadata spec

A URL to a multi-media attachment for the item. The file extensions GLTF, GLB, WEBM, MP4, M4V, OGV, and OGG are supported, along with the audio-only extensions MP3, WAV, and OGA.

#### `external_url` For backward compatibility with Opensea metadata spec

A URI  with additional information about the subject or content of the NFT.
