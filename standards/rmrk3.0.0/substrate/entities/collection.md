# Collection

This standard defines how **collection** of NFTs are minted. Collections are effectively immutable
after being created, with the exception of the issuer value, and the max value. See interactions at
the bottom of this page.

A collection is the context of one or more NFTs. For example, a Cryptokitty Generation 0 is part of
the "Generation 0 kitties" collection, and an ENS (Ethereum Name System) domain is part of the ENS
collection, while a POAP-EthParis NFT might be given to everyone who attended EthParis, and will be
part of the POAP master collection. Every NFT must have a parent context (collection) it belongs to.

## Collection Standard

A collection MUST adhere to the following standard.

```json
{
  "max": {
    "type": "number",
    "description": "How many NFTs will ever belong to this collection. 0 for infinite."
  },
  "issuer": {
    "type": "string",
    "description": "Issuer's address, e.g. CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp. Can be address different from minter to assign ownership to other entity on creation."
  },
  "symbol": {
    "type": "string",
    "description": "Ticker symbol by which to represent the token in wallets and UIs, e.g. ZOMB"
  },
  "id": {
    "type": "string",
    "description": "A collection is uniquely identified by at least the first four and last four bytes of the original issuer's pubkey, combined with the symbol through a dash `-`. This prevents anyone but the issuer from reusing the symbol, and prevents trading of fake NFTs with the same symbol. Example ID: 0aff6865bed3a66b-ZOMB."
  },
  "metadata": {
    "type": "string",
    "description": "HTTP(s) or IPFS URI. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  },
  "properties?": {
    "type": IProperties,
    "description": "A map of property object definitions that get inherited by every NFT in this collection"
  }
}
```

## On-chain Properties

A Collection can define a map of properties for NFTs in this collection to inherit. Propeties (previously Attributes)
defined this way override properties in the metadata of individual NFTs, but are overridden further
by properties of the same name on the NFT. The order of precedence is, from lowest to highest:

- Metadata Properties
- Collection-inherited Properties
- NFT's own Properties

Due to on-chain storage concerns and constraints, this function will usually be reserved only for
[mutable properties](../interactions/setproperty.md).

### Properties Format

```
export type IProperties = Record<string, IAttribute>;

export interface IAttribute {
  _mutation?: {
    allowed: boolean;
    with?: {
      opType: OP_TYPES;
      condition?: string;
    };
  };
  type: "array" | "object" | "int" | "float" | "boolean" | "datetime" | "string";
  value: any;
}
```

Inspired by 
[Enjin](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema),

Chain-level properties like these override a Collection's metadata properties.

## Metadata

A collection MUST have metadata to describe it and help visualization on various platforms. See full [Metadata spec here](./metadata.md)

```json
{
  "description": {
    "type": "string",
    "description": "Description of the collection as a whole. Markdown is supported."
  },
  "properties": {
    "type": "object",
    "description": "A map of JSON objects, inspired by Enjin's: https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema"
  },
  "externalUri": {
    "type": "string",
    "description": "HTTP or IPFS URL for finding out more about this project. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  },
  "mediaUri": {
    "type": "string",
    "description": "HTTP or IPFS URL to project's main image, in the vein of og:image. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  },
  "name": {
    "type": "string",
    "description": "Name of the collection, e.g. 'Dot Leap badges'."
  }
}
```

## Examples

Collection:

```json
{
  "max": 100,
  "issuer": "CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp",
  "symbol": "DLEP",
  "id": "0aff6865bed3a66b-DLEP",
  "metadata": "ipfs://ipfs/QmVgs8P4awhZpFXhkkgnCwBp4AdKRj3F9K58mCZ6fxvn3j"
}
```

Metadata:

```json
{
  "description": "Everyone who promoted [Dot Leap](https://dotleap.substack.com) via the in-email Tweet link is eligible.",
  "properties": {},
  "externalUri": "https://rmrk.app/registry/0aff6865bed3a66b-DLEP",
  "mediaUri": "ipfs://ipfs/QmYcWFQCY1bAZ7ffRggt367McMN5gyZjXtribj5hzzeCWQ"
}
```

## Interactions

- [CREATE](../interactions/create.md) - creates a new collection
- [CHANGEISSUER](../interactions/changeissuer.md) - changes issuer to another address
- [LOCK](../interactions/lock.md) - locks a collection's max number of NFTs to the current number of
  elements
