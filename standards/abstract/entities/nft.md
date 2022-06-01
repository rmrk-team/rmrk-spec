# NFT

An NFT is minted from a [Collection](collection.md). It's a unique digital asset. NFTs can be
mutable after being created, but most often are not.

NFTs can have [jsonlogic](https://jsonlogic.com/) to affect
[conditional rendering](#conditional-rendering) and [multiple resources](#resources) which can be
prioritized by the owner, and of which one or more can be based on a [Base](base.md). An NFT can own
other NFTs through two computed properties: `children` when parent, and `owner` / `rootowner` when
child.

## NFT Standard
- collection: Collection ID
- symbol: Identier, can be categorical
- transferable: Transferability.  Must be able to handle (1) is universally transferable, (2) is not transferrable, (3) transferable until a specified block, (4) transferable for a certain number of blocks after minting
- sn: Unique serial number
- metadata: HTTP(s) or IPFS URI

### On-chain Properties

An NFT can define a map of properties. Properties defined this way override properties in the metadata.

Due to on-chain storage concerns and constraints, this will usually be reserved only for
[mutable properties](../interactions/setproperty.md).

### Properties Format

Inspired by
[Enjin](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema),

```ts
export type IProperties = Record<string, IAttribute>;

export interface IAttribute {
  _mutation?: {
    allowed: boolean;
    with?: {
      opType: OP_TYPES;
      condition?: string;
    };
  };
  type: "array" | "object" | "int" | "float" | "number" | "string" | "boolean" | "datetime" | "royalty";
  value: any;
}
```

### Royalties

A royalty info is an extension of Attribute type and saved in a Properties object alongside other properties. Royalty property can only be updated by owner if the current owner of said NFT is also a collection issuer. `royalty` attribute cannot be mutated.
`receiver` is an Address of a royalty receiver and `royaltyPercentFloat` is a two-decimal floating point number, supporting 2.43% and similar values.

```ts
export interface IRoyaltyAttribute extends IAttribute {
  _mutation: IAttribute["_mutation"] & {
    allowed: false;
  };
  type: "royalty";
  value: {
    receiver: string;
    royaltyPercentFloat: number;
  };
}
```

Property of type `royalty` can only be mutated by rootowner of an NFT who is also a collection issuer.

NFT-level properties like these override a
[Collection's on-chain properties](collection.md#on-chain-properties).

### Computed fields

Computed fields are fields that are used in interactions, but are not explicitly set on their
entities. Computed fields are the result of applying a standardized calculation or merger formula to
specific fields. The NFT entity has the following computed fields, to be provided by
implementations:
- id: Unique identier of NFT
- children: list of children owned by NFT
- owner: account or NFT that owns this NFT
- rootowner: top-most account that ultimately owns the NFT
- resources: list of resources added to this NFT (both pending and accepted)
- priority: list of resource IDs, used for default rendering.  needs not be exhaustive (missing resources will be implied-added to the end of the list)
- logic: list of Logic objects, computed from SETPROPERTY

#### Children

Child object specs
- id: NFT id of the child object
- equipped: slot where equipped (or not equipped), can only equip to a slot if that NFT has a resource referencing this base+slot
- pending: if resource has been accepted

#### Owner and Rootowner

These values are originally computed from the [MINT](../interactions/mind.md) interaction, depending
on recipient. After minting, it is computed from [SEND](../interactions/send.md) interactions.

`rootowner` will ALWAYS be an account, whereas `owner` can also be an NFT ID in cases where an NFT
owns another NFT (see `children` above). An implementing toolset MUST take into consideration the
bubbling-up of ownership checks, so that if Owner O owns NFT A which owns NFT B which owns NFT C and
O issues a [BURN](../interactions/burn.md) to NFT C, it should just work. This is where `rootowner`
check should be utilized.

#### Resources and Resource

Resources can only be added to an NFT with the [RESADD](../interactions/resadd.md) interaction. They
cannot be included during minting. This is to maintain a separation of concerns.

A resource object is defined as such:
- id: Unique identifier
- base: Base ID (optional, required only for "slot" resource types)
- src: Media URI (optional)
- metadata: Metadata URI (optional)
- slot: Slot ID (optional, required only for "slot" resource types)
- license: Reference to license (optional, URI or identifier)
- thumb: Thumbnail URI (optional)

If the resource is a [Base](base.md), the `src` property is absent. In this case, the resource **can** also list the
_parts_ of this base which the NFT implements. A Base lists all the parts an NFT can possibly be composed of, and the NFT itself cherry-picks from that list of parts. If the list of parts is omitted, then it is assumed the NFT is composed of **all** the parts of the base. 

The resource can also pick a [Base's Theme](base.md#themes).

If the resource is Media, the `base` property is absent. Media `src` should be a URI like an IPFS
hash.

The `license` field, if present, should contain a link to a license (IPFS or static HTTP url), or an
identifier, like `RMRK_nocopy` or `ipfs://ipfs/someHashOfLicense`. This is a license transfering
copyright to the current owner. If omitted, it is assumed to be _Full rights with current owner_,
meaning the owner can use the art for any commercial or non commercial activity. This use must cease
with a transfer of ownership, so using an NFT one intends to sell as a company logo is probably not
a good idea.

If the resource has the `slot` property, it was designed to fit into a specific Base's slot.

If the resource has the `thumb` property, this will be a URI to a thumbnail of the given resource, that is lighter and faster to load but representative of this resource. `thumb` is what's shown in search result and inventory pages for then the given resource is at top [priority](../interactions/setpriority.md).

### Conflicting Base slot

A resource can have a `slot` property, which indicates which slot in a [base](../entities/base.md)
it's meant for. It can happen that a newly added resource targets the same slot on the same target
base, in which case the render needs to know which one to display as equipped into an NFT's base. In
cases like these, resource priority should be respected. Priority can be switched with the
[`SETPRIORITY`](../interactions/setpriority.md) interaction.

---

The metadata of a Resource see [Metadata Spec](./metadata.md)

Metadata is optional.

#### Priority

Priority defines the order in which resources are loaded.  Omitted resources are implied to go to the end of the list.  A user can change the priority order by using the [SETPRIORITY](../interactions/setpriority.md)
interaction. 

#### Logic

TBD

## Metadata Standard

See [Full Metadata Spec](./metadata.md)

## Standards Per Implementation

[Kusama NFT Entity](../../kusama/entities/nft.md)
