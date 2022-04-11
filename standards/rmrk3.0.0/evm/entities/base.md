# Base Entity (EVM)

A [Base](../../abstract/entities/base.md) in EVM is represented as a (TODO: Base in EVM?) 

Example:

TODO: Base in EVM example

## Caveats for EVM

TODO: Caveats for EVM Base entity

#### `id` (string)

TODO: ID caveats for EVM?

#### `equippable` (array)

TODO: what is the equippable "All" in EVM?

---

## Metadata Examples

### Using on-chain fields

TODO: Metadata on-chain example

### Using off-chain fields from metadata

```json
{
  "symbol": "kanaria_superbird",
  "parts": [
    {
      "id": "bg",
      "metadata": "ipfs://ipfs/hash"
    },
    ...
  ]
}
```

And the content of metadata would be 

```json
{
  "mediaUri": "ipfs://ipfs/QmZy8eRLhToqPk5154SJkTJfPD8AMnPAjBi6w1S61yNPrR/1F32B/1f32b_eyes.svg",
  "thumbnailUri": "ipfs://ipfs/QmZy8eRLhToqPk5154SJkTJfPD8AMnPAjBi6w1S61yNPrR/1F32B/1f32b_eyes_thumb.png",
  "type": "slot",
  "properties": {
    "mimeType": {
      "type": "string",
      "value": "image/svg+xml"
    },
    "z": {
      "type": "int",
      "value": 2
    }
  }
}
```
---

### Themes Example

#### On chain

TODO: Themes on-chain example for EVM

#### IPFS

```json
{
  "themes": {
    "default": "ipfs://ipfs/theme1hash",
    "sepia": "ipfs://ipfs/theme2hash"
  }
}
```

## Interactions

- [BASE](../interactions/base.md) - creates a base
- [EQUIPPABLE](../interactions/equippable.md) - changes the equippable collection set
- [CHANGEISSUER](../interactions/changeissuer.md) - changes the issuer
