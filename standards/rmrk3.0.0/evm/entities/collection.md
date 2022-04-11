# Collection Entity (EVM)

A [Collection](../../abstract/entities/collection.md) defines how **collection** of NFTs are minted.  This document describes EVM-specific examples and caveats for Collections.  See the [Abstract](../../abstract/entities/collection.md) for full specs.

## Collection Standard in EVM

TODO: EVM Collection standard.  This is from Kusama standard (or just delete?)
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

## Caveats

TODO: EVM caveats


### Properties Format

TODO: EVM Properties format

## Examples

Collection:

TODO: EVM collection example

## Interactions

- [CREATE](../interactions/create.md) - creates a new collection
- [CHANGEISSUER](../interactions/changeissuer.md) - changes issuer to another address
- [LOCK](../interactions/lock.md) - locks a collection's max number of NFTs to the current number of
  elements
