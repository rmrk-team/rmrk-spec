# Collection

This standard defines how **Collections** of NFTs are minted. Collections cannot be sent and are
effectively immutable after being created, with the exception of the issuer value. The current
issuer of the collection can assign a new issuer using a CHANGEISSUER interaction.

A collection is the context of one or more NFTs. For example, a Cryptokitty Generation 0 is an NFT
in the "Generation 0 kitties" collection, and an ENS (Ethereum Name System) domain is part of the
ENS collection, while a POAP-EthParis NFT might be given to everyone who attended EthParis, and will
be part of the POAP master collection. Every NFT must have a parent context it belongs to.

## Collection Standard

A collection MUST adhere to the following standard.

```json
{
  "name": {
    "type": "string",
    "description": "Name of the collection. Name must be limited to alphanumeric characters. Underscore is allowed as word separator. E.g. VALHELLO-ITEMS is NOT allowed. VALHELLO_ITEMS is allowed."
  },
  "max": {
    "type": "number",
    "description": "How many NFTs will ever belong to this collection. 0 for infinite."
  },
  "issuer": {
    "type": "string",
    "description": "Issuer's address, e.g. CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp. Can be address different from minter."
  },
  "symbol": {
    "type": "string",
    "description": "Ticker symbol by which to represent the token in wallets and UIs, e.g. ZOMB"
  },
  "id": {
    "type": "string",
    "description": "A collection is uniquely identified by at least the first four and last four bytes of the original issuer's pubkey, combined with the symbol. This prevents anyone but the issuer from reusing the symbol, and prevents trading of fake NFTs with the same symbol. Example ID: 0aff6865bed3a66b-ZOMB."
  },
  "metadata?": {
    "type": "string",
    "description": "HTTP(s) or IPFS URI. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  },
  "data?": {
    "type": "object",
    "description": "See Data"
  }
}
```

When either metadata or [data](#data) is present, the other is optional. Data takes precedence
always. Note that because metadata contains description, attributes, third party URLs, etc. it is
still recommended to include it alongside `data`.

### Data

The `data` object is composed of:

- protocol (strict, see Protocols below)
- data
- type (mime type)

#### Protocols

| Protocol  | Mime default           | Description                                                                                                                                    |
| --------- | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `ipfs`    | image/png              | Points to a directly interpretable resource, be it audio, video, code, or something else                                                       |
| `http(s)` | image/html             | Points to a directly interpretable resource, be it audio, video, code, or something else (not recommended for use)                             |
| `p5`      | application/javascript | Processing.js code                                                                                                                             |
| `js`      | application/javascript | Plain JS code                                                                                                                                  |
| `html`    | text/html              | HTML code, no need for `<html>` and `<body>`, can support dependencies but it's up to the author to prevent the dependencies from disappearing |
| `svg`     | image/svg+xml          | SVG image data                                                                                                                                 |
| `bin`     | n/a                    | binary, directly interpretable                                                                                                                 |

## Metadata

A collection SHOULD have metadata to describe it and help visualization on various platforms.

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
  "image_data": {
    "type": "string?",
    "description": "[OPTIONAL] Use only if you don't have the image field (they are mutually exclusive and image takes precedence). Raw base64 or SVG data for the image. If SVG, MUST start with <svg, if base64, MUST start with base64:"
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
