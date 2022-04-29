# MINT interaction (Abstract)

The MINT interaction creates an [NFT](../entities/nft.md) into a
[collection](../entities/collection.md).

Mint is only possible if this NFT's collection is unlimited or below its `max` value. You cannot
mint into a collection which has its number of elements == `max`, nor to a collection on which the
[LOCK](../interactions/lock.md) interaction was invoked.

## Standard
- `data`: full content of the NFT (e.g. as HTML-encoded JSON for Kusama)
- `recipient`: NFT ID (which becomes a parent of this NFT), or an account, minter if omitted (optional)

## Standards Per Implementation

[Kusama MINT](../../kusama/interactions/mint.md)

[Substrate MINT](../../substrate/interactions/mint.md)

[EVM MINT](../../evm/interactions/mint.md)