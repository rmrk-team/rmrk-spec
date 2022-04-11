[Root](../../)

# Base Entity (Substrate)

A [Base](../../abstract/entities/base.md) in Substrate is represented as a (TODO: Base in Substrate?) 

Example:

TODO: Base in Substrate example

## Caveats for Substrate

TODO: Caveats for Substrate Base entity

#### `id` (string)

TODO: ID caveats for Substrate?

#### `equippable` (array)

TODO: what is the equippable "All" in Substrate?

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

TODO: Themes on-chain example for Substrate

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
