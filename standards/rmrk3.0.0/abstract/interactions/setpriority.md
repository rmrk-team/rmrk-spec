# SETPRIORITY interaction (Abstract)

The `SETPRIORITY` interaction allows NFT owners to change the priority (rendering order) or their
[resources](../entities/nft.md#resources-and-resource).

## Format
- `data`: list of resource IDs

You **can** set priority with a resource ID that does not reference an existing resource. Invalid
resource IDs will simply be ignored.

## Standards Per Implementation

[Kusama SETPRIORITY](../../kusama/interactions/setpriority.md)

[Substrate SETPRIORITY](../../substrate/interactions/setpriority.md)

[EVM SETPRIORITY](../../evm/interactions/setpriority.md)