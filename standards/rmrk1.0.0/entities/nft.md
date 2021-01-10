# NFT

An NFT is a part of a collection. It's a unique digital asset. NFTs are immutable after being
created.

## NFT Standard

```json
{
  "collection": {
    "type": "string",
    "description": "Collection ID, e.g. 0aff6865bed3a66b-ZOMB"
  },
  "name": {
    "type": "string",
    "description": "Name of the NFT. Specific to NFT instance. E.g. Blue Zombie, Red Zombie. Name must be limited to alphanumeric characters. Underscore is allowed as word separator. E.g. LOVE-POTION is NOT allowed. LOVE_POTION is allowed."
  },
  "instance": {
    "type": "string",
    "description": "Instance ID, e.g. ZOMBBLUE"
  },
  "transferable": {
    "type": "number",
    "description": "If 1, item is transferable. If 0, item is not transferable (i.e. reputation token). If anything else, item will be transferable from that BLOCK onward, e.g. 1400000 means item can be traded after block 1400000."
  },
  "sn": {
    "type": "string",
    "description": "Serial number or issuance number of the NFT, padded so that its total length is 16, e.g. 0000000000000123"
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

### Computed fields

Computed fields are fields that are used in interactions, but are not explicitly set on their
entities. Computed fields are the result of applying a standardized calculation or merger formula to
specific fields. The NFT entity has the following computed fields, to be provided by
implementations:

```json
{
  "id": {
    "type": "computed",
    "description": "An NFT is uniquely identified by the combination of its minting block number, collection ID, its instance ID, and its serial number, e.g. 5193445-0aff6865bed3a66b-ZOMB-ZOMBBLUE-0000000000000001."
  }
}
```

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

#### Examples

#### A binary video

```json
data: {
  "protocol": "bin",
  "data": "AAAAIGZ0eXBpc29tAAACAGlzb21pc28yYXZjMW1wNDEAAAAIZnJlZQAQC0ttZGF0AQIUGRQmM...",
  "type": "video/mp4"
}
```

#### JavaScript

```json
data: {
  "protocol": "js",
  "data": "alert(\"Hello World\")",
  "type": "application/javascript"
}
```

The above, when interpreted, should immediately pop up a "Hello World" message.

#### NOTE:

- Implementers SHOULD prevent auto-render and auto-play of non-media resources, and in some cases of
  media resources, to protect end users from cross site scripting, crash attacks (malicious NFTs),
  and other nefarious actors.
- Implementers SHOULD expect the mime type to be there, but if missing, default to one common for
  the protocol, e.g. use `application/javascript` is mime type is missing but protocol is `js`.

## Metadata Standard

```json
{
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
  },
  "description": {
    "type": "string",
    "description": "Description of the collection as a whole. Markdown is supported."
  },
  "name": {
    "type": "string",
    "description": "Name of the NFT instance. If NAME is present in on-chain, it takes precedence over this one."
  },
  "attributes": {
    "type": "array",
    "description": "An Array of JSON objects, matching OpenSea's: https://docs.opensea.io/docs/metadata-standards#section-attributes"
  },
  "background_color": {
    "type": "string",
    "description": "Background color of the item. Must be a six-character hexadecimal without #."
  },
  "animation_url": {
    "type": "string",
    "description": "HTTP or IPFS URL (format MUST be ipfs://ipfs/HASH) for an animated image of the item. GLTF, GLB, WEBM, MP4, M4V, and OGG are supported, and when using IPFS type MUST be appended, separated by colon, e.g. ipfs://ipfs/SOMEHASH:webm."
  },
  "youtube_url": {
    "type": "string",
    "description": "URL to Youtube video about this NFT"
  }
}
```

## Examples

NFT:

```json
{
  "collection": "0aff6865bed3a66b-DLEP",
  "name": "DL15",
  "transferable": 1,
  "sn": "0000000000000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4"
}
```

Metadata:

```json
{
  "external_url": "https://rmrk.app/registry/0aff6865bed3a66b-DLEP",
  "image": "ipfs://ipfs/QmSY3VzdNdAphEs51GW9QMAUotaX3Rf6WeGQkvPPVhEQ3B",
  "description": "Everyone who promoted Dot Leap via the in-email link in edition 15",
  "name": "DL15",
  "attributes": [],
  "background_color": "ffffff"
}
```

NFT:

```json
{
  "collection": "0aff6865bed3a66b-DLEP",
  "name": "DL16",
  "transferable": 1,
  "sn": "0000000000000001",
  "metadata": "ipfs://ipfs/QmR3EB16GANjYbT82jueyMbv7ewrwVAogmB4fgbtUrRPLb"
}
```

Metadata:

```json
{
  "external_url": "https://rmrk.app/registry/0aff6865bed3a66b-DLEP",
  "image": "ipfs://ipfs/QmaCxd3omNNvjeVvzgn5gSjARDR4442WBtBAcZN7xEddeL",
  "description": "Everyone who promoted Dot Leap via the in-email link in edition 16",
  "name": "DL16",
  "attributes": [],
  "background_color": "ffffff"
}
```
