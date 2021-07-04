# NftClass

This standard defines how **classes** of NFTs are minted. Classes (previously Collections) are
effectively immutable after being created, with the exception of the issuer value, and the max
value. See interactions at the bottom of this page.

A class is the context of one or more NFTs. For example, a Cryptokitty Generation 0 is part of the
"Generation 0 kitties" class, and an ENS (Ethereum Name System) domain is part of the ENS class,
while a POAP-EthParis NFT might be given to everyone who attended EthParis, and will be part of the
POAP master class. Every NFT must have a parent context (class) it belongs to.

The change from Collection to Class was made to avoid confuction between Collections and Bundles and
to be more in sync with
[Parity's Uniques pallet](https://github.com/paritytech/substrate/tree/master/frame/uniques).

## NftClass Standard

A class MUST adhere to the following standard.

````json
{
  "max": {
    "type": "number",
    "description": "How many NFTs will ever belong to this class. 0 for infinite."
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
    "description": "A class is uniquely identified by at least the first four and last four bytes of the original issuer's pubkey, combined with the symbol through a dash `-`. This prevents anyone but the issuer from reusing the symbol, and prevents trading of fake NFTs with the same symbol. Example ID: 0aff6865bed3a66b-ZOMB."
  },
  "metadata": {
    "type": "string",
    "description": "HTTP(s) or IPFS URI. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  }
}
```                                                                                        |

## Metadata

A class MUST have metadata to describe it and help visualization on various platforms.

```json
{
  "description": {
    "type": "string",
    "description": "Description of the class as a whole. Markdown is supported."
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
    "description": "Name of the class, e.g. 'Dot Leap badges'."
  }
}
```

## Examples

Class:

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

- [CREATE](../interactions/create.md) - creates a new class
- [CHANGEISSUER](../interactions/changeissuer.md) - changes issuer to another address
- [LOCK](../interactions/lock.md) - locks a class' max number of NFTs to the current number of elements
````
