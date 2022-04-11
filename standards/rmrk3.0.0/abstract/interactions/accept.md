# ACCEPT

The ACCEPT interaction allows the owner of an [NFT](../entities/nft.md) to accept:

- a pending resource added by [RESADD](resadd.md), or to accept
- a child NFT that is being [SENT](send.md) into an owned parent NFT

## Implications for Resources

If the resource being accepted is the first resource of this NFT, then this operations results in
the NFT being given a `priority` property with the value of `[{ID}]` where `{ID}` is the ID of the
resource, e.g. `V1i6B` makes `priority` into `["V1i6B"]`.

## Pending

A resource that is added to an NFT via `RESADD` enters a pending state and MUST be accepted with an
`ACCEPT` unless the owner of the NFT is the same account as the issuer of the collection, in which
case it is automatically accepted even without a `ACCEPT` ever being issued.

A child NFT that is sent to another NFT via `SEND` enters a pending state and MUST be accepted with
an `ACCEPT` unless the `rootowner` of both NFTs matches.

## Removal of resources

It is not possible to remove resources accepted in the past. This prevents art rug-pulls.

## Standards

[Kusama ACCEPT](../../kusama/accept.md)

[Substrate ACCEPT](../../substrate/accept.md)

[EVM ACCEPT](../../evm/accept.md)