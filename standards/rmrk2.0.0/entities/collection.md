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
  "attributes?": {
    "type": Attribute[],
    "description": "An array of attribute definitions that get inherited by every NFT in this collection"
  }
}
```

## On-chain Attributes

A Collection can define an array of attributes for NFTs in this collection to inherit. Attributes
defined this way override attributes in the metadata of individual NFTs, but are overridden further
by attributes of the same name on the NFT. The order of precedence is, from lowest to highest:

- Metadata Attribute
- Collection-inherited Attribute
- NFT's own Attribute

Due to on-chain storage concerns and constraints, this function will usually be reserved only for
[mutable attributes](../interactions/setattribute.md).

### Attribute Format

The same as
[OpenSea Attribute format](https://docs.opensea.io/docs/metadata-standards#section-attributes) with
an extra `mutable` flag and optional `mutator` and `bubble_on_equip` values, e.g.:

Example:

```json
{
  "mutable": true,
  "trait_type": "Hair color",
  "value": "blue",
  "mutator": "owner",
  "bubble_on_equip": true
}
```

`mutator` can be `owner` (default) or `issuer`. This defines who can mutate this item's attribute:
the current owner of the NFT, or the issuer of the collection. `bubble_on_equip` defines whether
this attribute is local (default: `false`), or global (`true`), i.e. bubbles up to its parent.

Example of `mutator:owner` and `bubble_on_equip:true` is a name-change crystal where renaming a
`name` attribute on an NFT applies the new value to the `name` attribute of the parent. Example of
`mutator:issuer` and `bubble_on_equip:false` is triggering some kind of condition like an NFT bonus
having expired, or Ragnarok on Viking-themed NFTs in a common collection, etc.

## Metadata

A collection MUST have metadata to describe it and help visualization on various platforms.

```json
{
  "description": {
    "type": "string",
    "description": "Description of the collection as a whole. Markdown is supported."
  },
  "attributes": {
    "type": "array",
    "description": "An Array of JSON objects, matching OpenSea's: https://docs.opensea.io/docs/metadata-standards#section-attributes"
  },
  "external_url": {
    "type": "string",
    "description": "HTTP or IPFS URL for finding out more about this project. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  },
  "image": {
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
  "name": "Dot Leap Early Promoters",
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
  "attributes": [],
  "external_url": "https://rmrk.app/registry/0aff6865bed3a66b-DLEP",
  "image": "ipfs://ipfs/QmYcWFQCY1bAZ7ffRggt367McMN5gyZjXtribj5hzzeCWQ"
}
```

## Interactions

- [CREATE](../interactions/create.md) - creates a new collection
- [CHANGEISSUER](../interactions/changeissuer.md) - changes issuer to another address
- [LOCK](../interactions/lock.md) - locks a collection's max number of NFTs to the current number of
  elements

```

```
