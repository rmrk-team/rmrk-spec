# NFT

An NFT is minted from a [Collection](collection.md). It's a unique digital asset. NFTs can be
mutable after being created, but most often are not.

NFTs can have [jsonlogic](https://jsonlogic.com/) to affect
[conditional rendering](#conditional-rendering) and [multiple resources](#resources) which can be
prioritized by the owner, and of which one or more can be based on a [Base](base.md). An NFT can own
other NFTs through two computed properties: `children` when parent, and `owner` / `rootowner` when
child.

## NFT Standard

```json
{
  "collection": {
    "type": "string",
    "description": "Collection ID, e.g. 0aff6865bed3a66b-ZOMB"
  },
  "symbol": {
    "type": "string",
    "description": "e.g. ZOMBBLUE. Must be limited to alphanumeric characters, no dash (-). Underscore is allowed as word separator. E.g. LOVE-POTION is NOT allowed. LOVE_POTION is allowed."
  },
  "transferable": {
    "type": "number",
    "description": "If 1, item is transferable. If 0, item is not transferable (i.e. reputation token). If positive integer, item will be transferable from that BLOCK onward, e.g. 1400000 means item can be traded after block 1400000. If negative integer, item will be transferable for that many blocks and then become non-transferable."
  },
  "sn": {
    "type": "string",
    "description": "Serial number or issuance number of the NFT, padded so that its total length is 8, e.g. 00000123. Should be sequential but can be arbitrary - the `max` propery of an NFT's collection only cares about the total number of NFTs, not their serial numbers."
  },
  "metadata": {
    "type": "string",
    "description": "HTTP(s) or IPFS URI. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  }
}
```

### On-chain Attributes

An NFT can define an array of attributes. Attributes defined this way override attributes in the
metadata.

Due to on-chain storage concerns and constraints, this will usually be reserved only for
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

NFT-level attributes like these override a
[Collection's on-chain attributes](collection.md#on-chain-attributes).

### Computed fields

Computed fields are fields that are used in interactions, but are not explicitly set on their
entities. Computed fields are the result of applying a standardized calculation or merger formula to
specific fields. The NFT entity has the following computed fields, to be provided by
implementations:

```json
{
  "id": {
    "type": "computed",
    "description": "An NFT is uniquely identified by the combination of its minting block number, collection ID, its instance ID, and its serial number, e.g. 5193445-0aff6865bed3a66b-ZOMB-ZOMBBLUE-00000001."
  },
  "children": {
    "type": Child[],
    "description": "Array of Child objects"
  },
  "owner": {
    "type": "string",
    "description": "Either account which owns the NFT or NFT ID of NFT that owns this NFT. Computed from SEND interactions after the initial MINT."
  },
  "rootowner": {
    "type": "string",
    "description": "Account which owns the top-most NFT in this stack. Example, Bob owns nft-a which owns nft-b. Root owner of nft-b is Bob."
  },
  "resources": {
    "type": Resource[],
    "description": "An array of Resource objects, computed from RESADD interactions."
  },
  "priority": {
    "type": "string[]",
    "description": "An array of resource IDs. Changing this changes the order of default rendering. Example: ['skdlj','wyeiu']. Changes are made with SETATTRIBUTE, and any resource not explicitly mentioned in the priority list is implied to go to the end of the list."
  },
  "logic": {
    "type": Logic[],
    "description": "Set of Logic objects, computed from SETATTRIBUTE interactions."
  }
}
```

#### ID

Example id: `5105000-0aff6865bed3a66b-DLEP-DLEP15-00000001`.

When processing NFTs and their interactions, tools MUST explode the NFT by `-` and if the number of
fragments is anything other than 5, the remark should be discarded as invalid:

- element 0 is the block in which the NFT was minted.
- elements 1 and 2 together make the [collection ID](collection.md).
- element 3 is the instance ID of the NFT (its symbol).
- element 4 is the serial number of the current NFT instance.

#### Children

`children` is an array of Child objects:

```ts
export interface NFTChild {
  id: string;
  equipped: string;
  pending: boolean;
}
```

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

#### Owner and Rootowner

These values are originally computed from the [MINT](../interactions/mind.md) interaction, depending
on recipient. After minting, it is computed from [SEND](../interactions/send.md) interactions.

`rootowner` will ALWAYS be an account, whereas `owner` can also be an NFT ID in cases where an NFT
owns another NFT (see `children` above). An implementing toolset MUST take into consideration the
bubbling-up of ownership checks, so that if Owner O owns NFT A which owns NFT B which owns NFT C and
O issues a [CONSUME](../interactions/consume.md) to NFT C, it should just work. This is where
`rootowner` check should be utilized.

#### Resources and Resource

Resources can only be added to an NFT with the [RESADD](../interactions/resadd.md) interaction. They
cannot be included during minting. This is to maintain a separation of concerns.

A resource object is defined as such:

```json
{
  "id": "nanoid-of-resource",
  "base?": "base-uri",
  "src?": "media-uri",
  "metadata?": "metadata-uri",
  "slot?": "baseslot",
  "license?": "url-or-identifier"
}
```

`id` is a 5-character string of _reasonable uniqueness_. The combination of base ID and resource id
should be unique across the entire RMRK ecosystem which, if entropy of the Base ID is combined with
a pseudo-randomly generated nanoid of a resource, is easy to satisfy. For JS-based implementations,
we recommend using the [nanoid](https://www.npmjs.com/package/nanoid) package.

If the resource is a [Base](base.md), the `src` property is absent. Base should be a
[Base ID computed field](base.md#computed-fields). In this case, the resource **can** also list the
_parts_ of this base which the NFT implements. A Base lists all the parts an NFT can possibly be
composed of, and the NFT itself cherry-picks from that list of parts. If the list of parts is
omitted, then it is assumed the NFT is composed of **all** the parts of the base. Example:

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

The resource can also pick a [Base's Theme](base.md#themes), like so:

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

If the `theme` value is omitted, it is implied to be `default`. To learn more about themes, see the
[THEMEADD](../interactions/themeadd.md) interaction.

If the resource is Media, the `base` property is absent. Media `src` should be a URI like an IPFS
hash.

The `license` field, if present, should contain a link to a license (IPFS or static HTTP url), or an
identifier, like `RMRK_nocopy` or `ipfs://ipfs/someHashOfLicense`. This is a license transfering
copyright to the current owner. If omitted, it is assumed to be _Full rights with current owner_,
meaning the owner can use the art for any commercial or non commercial activity. This use must cease
with a transfer of ownership, so using an NFT one intends to sell as a company logo is probably not
a good idea.

If the resource has the `slot` property, it was designed to fit into a specific Base's slot. The
baseslot will be composed of two dot-delimited values, like so:
`"base-4477293-kanaria_superbird.machine_gun_scope"`. This means: "This resource is compatible with
the `machine_gun_scope` slot of base `base-4477293-kanaria_superbird`. If the NFT with this resource
becomes a child of an NFT which has `base-4477293-kanaria_superbird` as its base, it will be
equippable into that NFT (see `children` above).

### Conflicting Base slot

A resource can have a `slot` property, which indicates which slot in a [base](../entities/base.md)
it's meant for. It can happen that a newly added resource targets the same slot on the same target
base, in which case the render needs to know which one to display as equipped into an NFT's base. In
cases like these, resource priority should be respected. Priority can be switched with the
[`SETATTRIBUTE`](setattribute) interaction.

As an example:

- a Kanaria bird has a `left_hand` slot on its `base1` base
- an NFT `excalibur` has a resource targeting `base1.left_hand`
- if equipped, the bird shows the `excalibur`'s `base1.left_hand` resource in the `base1.left_hand`
  slot
- the original artist notices a glitch, or there's a copyright issue on the original drawing of the
  `base1.left_hand` resource of `excalibur` and the artist makes a new rendition
- a new resource is added to `excalibur` targeting the same `base1.left_hand` slot
- the armed bird still shows the old resource until the owner of the `excalibur` NFT calls
  [`SETATTRIBUTE`](setattribute) on the `priority` field, changing the priority of the rendering

---

The metadata of a Resource:

```json
{
  "description?": {
    "type": "string",
    "description": "Description of the resource. Markdown is supported."
  },
  "external_url?": {
    "type": "string",
    "description": "HTTP or IPFS URL for finding out more about this project or linking to its author. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  },
  "name?": {
    "type": "string",
    "description": "Name of resource."
  }
}
```

Metadata is optional and should only be rendered and fetched by implementers on-demand. It is meant
to serve more as "credits" than anything else, in cases when several people worked on different
aspects of the same NFT project and different resources inside it.

Example of complete resources array:

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

Priority defines the order in which resources are loaded. This is very useful when an NFT has
several resources of the same type, but wants one to be the default. For example, a
[Kanaria](https://kanaria.rmrk.app) bird has a secondary and primary artwork. One is a resource with
ID `furio`, the other `wyqyc`. By changing priority to ['wyqyc'], the second resource is rendered
before the first in various visual applications like marketplaces and galleries.

```json
"priority": ["wyqyc", "furio"]
```

Because omitted resources are implied to go to the end of the list, the following is also valid:

```json
"priority": ["wyqyc"]
```

A user can change the priority order by using the [SETPRIORITY](../interactions/setpriority.md)
interaction. Example, to change priority of resource loading on NFT
`438637-0aff6865bed3a66b-KANS-oiyhi24yr28i7g4f` from `["wyqyc", "furio"]` to `["furio", "wyqyc"]`:

```
rmrk::SETPRIORITY::2.0.0::438637-0aff6865bed3a66b-KANS-oiyhi24yr28i7g4f::%5B%22wyqyc%22%2C%20%22furio%22%5D
```

#### Logic

TBD

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
    "description": "Description of the nft. Markdown is supported."
  },
  "name": {
    "type": "string",
    "description": "Name of the NFT instance."
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
  "transferable": 1,
  "sn": "00000001",
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
  "transferable": 1,
  "sn": "00000001",
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
