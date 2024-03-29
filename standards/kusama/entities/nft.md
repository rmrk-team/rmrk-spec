[Root](../)

# NFT Entity (Kusama)

This document describes Kusama-specific examples and caveats for [NFT](../../abstract/entities/nft.md).  See the [Abstract](../../abstract/entities/nft.md) for full specs.

## NFT Standard Caveats (Kusama)
- symbol: Must be limited to alphanumeric characters, no dash (-). Underscore is allowed as word separator. E.g. LOVE-POTION is NOT allowed. LOVE_POTION is allowed.
- transferable: If 1, item is transferable. If 0, item is not transferable (i.e. reputation token). If positive integer, item will be transferable from that BLOCK onward, e.g. 1400000 means item can be traded after block 1400000. If negative integer, item will be transferable for that many blocks and then become non-transferable.
- sn: Serial number or issuance number of the NFT, padded so that its total length is 8, e.g. 00000123. Should be sequential but can be arbitrary - the `max` propery of an NFT's collection only cares about the total number of NFTs, not their serial numbers.
- metadata: If IPFS, MUST be in the format of ipfs://ipfs/HASH

#### ID (Kusama)

Example id: `5105000-0aff6865bed3a66b-DLEP-DLEP15-00000001`.

When processing NFTs and their interactions, tools MUST explode the NFT by `-` and if the number of
fragments is anything other than 5, the remark should be discarded as invalid:

- element 0 is the block in which the NFT was minted.
- elements 1 and 2 together make the [collection ID](collection.md).
- element 3 is the instance ID of the NFT (its symbol).
- element 4 is the serial number of the current NFT instance.

#### Children (Kusama)

Example of fully consolidated `children` object:

```json
"children": [
  {
    "id": "5105000-0aff6865bed3a66b-DLEP-DL15-00000001",
    "equipped": "",
    "pending": false,
  },
  {
    "id": "5105000-0aff6865bed3a66b-DLEP-DL15-00000002",
    "equipped": "",
    "pending": true,
  },
  {
    "id": "5105000-0aff6865bed3a66b-DLEP-DL15-00000003",
    "equipped": "base_1.slot_1",
    "pending": false,
  }
]
```

The `equipped` property will change when computed from the [EQUIP](../interactions/equip.md)
interaction. It will will be composed of two dot-delimited values, like so:
`"base-4477293-kanaria_superbird.machine_gun_scope"`. This means: the child is now equipped into
this slot. The value of `baseslot` can change from `""` to
`"base-4477293-kanaria_superbird.machine_gun_scope"` ONLY if one of this child NFT's resources has
this value as a `slot` property (see `resources` below) and additional explained in the
[EQUIP interaction](../interactions/equip.md).

#### Resources (Kusama)

`id` is a 5-character string of _reasonable uniqueness_. The combination of base ID and resource id
should be unique across the entire RMRK ecosystem which, if entropy of the Base ID is combined with
a pseudo-randomly generated nanoid of a resource, is easy to satisfy. For JS-based implementations,
we recommend using the [nanoid](https://www.npmjs.com/package/nanoid) package.

Resources Example:

```json
    "resources": [
      {
          "id": "V1StG",
          "base": "some-base-id",
          "parts": ["left_wing_front", "left_wing_back", "gem_slot_1"]
      },
      {
          "id": "Z5i6B",
          "src": "hash-of-guest-bird-art-file",
          "metadata": "hash-of-metadata-with-credits"
      }
    ]
```

The first resource references a base, and picks 3 parts from it - two appear to be fixed, and one
appears to be a slot. Now this NFT can equip other NFTs into the slot, and will also render
`left_wing_front` and `left_wing_back`.

[Base's Theme](base.md#themes) example:

```json
    "resources": [
      {
          "id": "V1StG",
          "base": "some-base-id",
          "parts": ["left_wing_front", "left_wing_back", "gem_slot_1"],
          "theme": "yellow"
      }
    ]
```

If the `theme` value is omitted, it is implied to be using the SVG's fallback values, included in
the SVG itself as data attributes. To learn more about themes, see the
[THEMEADD](../interactions/themeadd.md) interaction.

If the resource has the `slot` property, it was designed to fit into a specific Base's slot. In Kusama, the
baseslot will be composed of two dot-delimited values, like so:
`"base-4477293-kanaria_superbird.machine_gun_scope"`. This means: "This resource is compatible with
the `machine_gun_scope` slot of base `base-4477293-kanaria_superbird`. If the NFT with this resource
becomes a child of an NFT which has `base-4477293-kanaria_superbird` as its base, it will be
equippable into that NFT (see `children` above).

### Conflicting Base slot

Kusama conflict example:

- a Kanaria bird has a `left_hand` slot on its `base1` base
- an NFT `excalibur` has a resource targeting `base1.left_hand`
- if equipped, the bird shows the `excalibur`'s `base1.left_hand` resource in the `base1.left_hand`
  slot
- the original artist notices a glitch, or there's a copyright issue on the original drawing of the
  `base1.left_hand` resource of `excalibur` and the artist makes a new rendition
- a new resource is added to `excalibur` targeting the same `base1.left_hand` slot
- the armed bird still shows the old resource until the owner of the `excalibur` NFT calls
  [`SETPRIORITY`](../interactions/setpriority.md) on the `priority` field, changing the priority of
  the rendering

---

Kusama example of complete resources array:

```json
    "resources": [
      {
          "id": "V1StG",
          "base": "hash-of-base-svg.json"
      },
      {
          "id": "tGXR8",
          "src": "hash-of-metadata-containing-guest-bird-art",
          "slot": "base-4477293-kanaria_superbird.wing_1_slot"
      },
      {
          "id": "Z5i6B",
          "src": "hash-of-metadata-guest-bird-art-with-jetpack",
          "metadata": "hash-of-metadata-with-credits"
      }
    ]
```

#### Priority

Kusama priority example: A [Kanaria](https://kanaria.rmrk.app) bird has a secondary and primary artwork. One is a resource with ID `furio`, the other `wyqyc`. By changing priority to ['wyqyc'], the second resource is rendered before the first in various visual applications like marketplaces and galleries.

```json
"priority": ["wyqyc", "furio"]
```

Because omitted resources are implied to go to the end of the list, the following is also valid:

```json
"priority": ["wyqyc"]
```

A user can change the priority order by using the [SETPRIORITY](../interactions/setpriority.md)
interaction.

## Kusama NFT Examples

NFT:

```json
{
  "collection": "0aff6865bed3a66b-DLEP",
  "transferable": 1,
  "sn": "00000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4"
}
```

Metadata:

```json
{
  "externalUri": "https://rmrk.app/registry/0aff6865bed3a66b-DLEP",
  "mediaUri": "ipfs://ipfs/QmSY3VzdNdAphEs51GW9QMAUotaX3Rf6WeGQkvPPVhEQ3B",
  "description": "Everyone who promoted Dot Leap via the in-email link in edition 15",
  "name": "DL15",
  "properties": {}
}
```

NFT:

```json
{
  "collection": "0aff6865bed3a66b-DLEP",
  "transferable": 1,
  "sn": "00000001",
  "metadata": "ipfs://ipfs/QmR3EB16GANjYbT82jueyMbv7ewrwVAogmB4fgbtUrRPLb"
}
```

Metadata:

```json
{
  "externalUri": "https://rmrk.app/registry/0aff6865bed3a66b-DLEP",
  "mediaUri": "ipfs://ipfs/QmaCxd3omNNvjeVvzgn5gSjARDR4442WBtBAcZN7xEddeL",
  "description": "Everyone who promoted Dot Leap via the in-email link in edition 16",
  "name": "DL16",
  "properties":  {
    "nickname": {
      "_mutation":  {
        "allowed": true,
        "with": {
          "condition": "d43593c715a56da27d-KANARIAGEMS",
          "opType": "BURN"
        }
      },
      "type": "string",
      "value": "foo"
    }
  }
}
```
