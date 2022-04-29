# RESADD interaction (Abstract)

The RESADD interaction allows the `issuer` of a [collection](../entities/collection.md) to suggest a
new resource for an [NFT](../entities/nft.md) in that collection. If the `issuer` is also the
`owner` of this NFT, this interaction also counts as a [ACCEPT](accept.md) automatically.

This is useful for adding new rendering approaches to existing art, for adding alternative
reproductions to a certain piece of media, and more. As an example, consider an NFT which is a book.
In its raw form, it can just be a PDF. But we can also `RESADD` a resource which is a
high-resolution cover, and one which is an MP3 file. Now the book can be loaded into a marketplace
like Opensea, showing the cover, into Audible and play back the audio version, or into
[Singular](https://singular.rmrk.app) to be read as a PDF.

## Implications

If the resource being added is the first resource of this NFT, and it is accepted, then this
operation results in the NFT being given a `priority` property with the value of `[0]`, meaning the
first resource of this NFT is highest priority.

## Pending

A resource that is added to an NFT via `RESADD` enters a pending state and MUST be accepted with a
`ACCEPT` unless the owner of the NFT is the same account as the issuer of the collection, in which
case it is automatically accepted.

## Spam

It is possible to spam resources on an NFT that forever take up a resource pending queue. A way to
permanently reject resources may be added later if this proves to be the case, but the official
recommendation towards implementers right now is to ignore resources that have been pending for more
than a month.

## Conflicting Base slot

A resource can have a `slot` property, which indicates which slot in a [base](../entities/base.md)
it's meant for. It can happen that a newly added resource targets the same slot on the same target
base, in which case the render needs to know which one to display as equipped into an NFT's base. In
cases like these, resource priority should be respected. Priority can be switched with the
[`SETPRIORITY`](setpriority.md) interaction.

As an example:

- a Kanaria bird has a `left_hand` slot
- an NFT `excalibur` has a resource targeting `left_hand`
- if equipped, the bird shows the `excalibur`'s `left_hand` resource in the `left_hand` slot
- the original artist notices a glitch, or there's a copyright issue on the original drawing of the
  `left_hand` resource of `excalibur` and the artist makes a new rendition
- a new resource is added to `excalibur` targeting the same `left_hand` slot
- the armed bird still shows the old resource until the owner of the `excalibur` NFT calls
  [`SETPRIORITY`](setpriority.md) interaction, changing the priority of the rendering

## Standard
- `id`: [NFT](../entities/nft.md) ID
- `data`: Resource, formatted according to the standard in [NFT](../entities/nft.md) under the Resources section

## Standards Per Implementation

[Kusama RESADD](../../kusama/interactions/resadd.md)
