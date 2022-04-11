[Root](../../)

# Collection Entity (Kusama)

A [Collection](../../abstract/entities/collection.md) defines how **collection** of NFTs are minted.  This document describes Kusama-specific examples and caveats for Collections.  See the [Abstract](../../abstract/entities/collection.md) for full specs.

## Collection Standard in Kusama

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

- id: A collection is uniquely identified by at least the first four and last four bytes of the original issuer's pubkey, combined with the symbol through a dash `-`. This prevents anyone but the issuer from reusing the symbol, and prevents trading of fake NFTs with the same symbol. Example ID: 0aff6865bed3a66b-ZOMB.
- metadata: If IPFS, MUST be in the format of ipfs://ipfs/HASH


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

## Interactions

- [CREATE](../interactions/create.md) - creates a new collection
- [CHANGEISSUER](../interactions/changeissuer.md) - changes issuer to another address
- [LOCK](../interactions/lock.md) - locks a collection's max number of NFTs to the current number of
  elements
