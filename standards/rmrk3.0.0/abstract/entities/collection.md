# Collection Entity (Abstract)

This standard defines how **collection** of NFTs are minted. Collections are effectively immutable
after being created, with the exception of the issuer value, and the max value. See interactions at
the bottom of this page.

A collection is the context of one or more NFTs. For example, a Cryptokitty Generation 0 is part of
the "Generation 0 kitties" collection, and an ENS (Ethereum Name System) domain is part of the ENS
collection, while a POAP-EthParis NFT might be given to everyone who attended EthParis, and will be
part of the POAP master collection. Every NFT must have a parent context (collection) it belongs to.

## Collection Standard

A collection MUST adhere to the following standard.
- max: How many NFTs will ever belong to this collection. 0 for infinite.
- issuer: Issuer's address (can be different than minter)
- symbol: Ticker symbol by which to represent the token in wallets and UIs (e.g. "ZOMB")
- id: Unique identifier
- metadata: HTTP(s) or IPFS URI for metadata
- properties: A map of property object definitions that get inherited by every NFT in this collection (Optional)

## On-chain Properties

A Collection can define a map of properties for NFTs in this collection to inherit. Propeties defined this way override properties in the metadata of individual NFTs, but are overridden further by properties of the same name on the NFT. The order of precedence is, from lowest to highest:

- Metadata Properties
- Collection-inherited Properties
- NFT's own Properties

Due to on-chain storage concerns and constraints, this function will usually be reserved only for
[mutable properties](../interactions/setproperty.md).

### Properties Format

Inspired by 
[Enjin](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema),

Chain-level properties like these override a Collection's metadata properties.

## Metadata

A collection MUST have metadata to describe it and help visualization on various platforms. See full [Metadata spec here](./metadata.md)

- description: Description of the collection as a whole.
- properties: A map of JSON objects, inspired by Enjin's: https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema
- externalUri: HTTP or IPFS URL for finding out more about this project
- mediaUri: HTTP or IPFS URL to project's main image, in the vein of og:image
- name: Name of the collection

## Interactions

- [CREATE](../interactions/create.md) - creates a new collection
- [CHANGEISSUER](../interactions/changeissuer.md) - changes issuer to another address
- [LOCK](../interactions/lock.md) - locks a collection's max number of NFTs to the current number of
  elements

## Standards Per Implementation

[Kusama Collection Entity](../../kusama/entities/collection.md)

[Substrate Collection Entity](../../substrate/entities/collection.md)

[EVM Collection Entity](../../evm/entities/collection.md)