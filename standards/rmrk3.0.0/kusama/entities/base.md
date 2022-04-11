# Base Entity (Kusama)

A [Base](../../abstract/entities/base.md) in Kusama is represented as a JSON object with following properties: `symbol`, `type`, `themes` and `parts`. There is an implied
`issuer` field, matching the address that created the base.  The `id` field which is dynamically
generated based on the `symbol` and some other values (see Computed Properties below).

Example:

```json
{
    "symbol": "kanaria_superbird",
    "type": "svg",
    "parts": [
      {
          "id": "bg",
          "src": "ipfs://ipfs/hash",
          "thumb": "ipfs://ipfs/hash",
          "type": "slot",
          "equippable": ["collection_1", "collection_2"],
          "z": 3
      },
      {
          "id": "gem_1",
          "src": "ipfs://ipfs/hash",
          "type": "fixed",
          "z": 4
      },
      {
          "id": "wing_1_back",
          "src": "ipfs://ipfs/hash",
          "metadata": "ipfs://ipfs/hash"
      },
      {
          "id": "wing_1_front",
          "metadata": "ipfs://ipfs/hash2"
      }
    ]
}
```

## Caveats for Kusama

#### symbol (string)

It can be any string value, but must not use dashes or dots. `kanaria-epic-birds` is not OK, but `kanaria_epic_birds` is.

This is because the computed `id` property uses dashes `-` to combine additional elements, adding
uniqueness to the ID, and because dots `.` are used for `slot` addressing.

### Computed Fields

#### `issuer` (string Address)

Chain-specific address of the original creator of the Base, or address of new issuer if issuer was changed with the CHANGEISSUER interaction.

#### `id` (string)

A Base is uniquely identified by the combination of the word `base`, its minting block number, and user provided symbol during Base creation, glued by dashes `-`, e.g. base-4477293-kanaria_superbird.

#### `equippable` (array)

The "All" representation of equippable is the wildcard '*'.

---

## Metadata Examples

### Using on-chain fields

```json
{
  "symbol": "kanaria_superbird",
  "parts": [
    {
      "id": "bg",
      "src": "ipfs://ipfs/hash",
      "thumb": "ipfs://ipfs/hash",
      "type": "slot",
      "equippable": ["collection_1", "collection_2"],
      "z": 3
    },
    ...
  ]
}
```


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

```json
{
  "themes": {
    "default": {
      "primary_color": "yellow",
      "secondary_color": "darkyellow"
    },
    "sepia": {
      "primary_color": "0x47822a",
      "secondary_color": "0x331213"
    }
  }
}
```

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
