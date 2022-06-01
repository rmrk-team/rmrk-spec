# BURN interaction (Abstract)

The BURN interaction burns an [NFT](../entities/nft.md) for a specific purpose.

This is useful when NFTs are spendable like with in-game potions, one-time votes in DAOs, or concert
tickets.

You can only BURN an existing NFT (one that has not been burned yet) and only if you own it. If
another NFT owns the NFT, the owner check bubbles up until the real owner is found and can be
checked (see `rootowner` computed property of the [NFT](../entities/nft.md)).

If an NFT that contains other NFTs is being burnt, the **owned NFTs are also burned**. Implementers
should implement UI warnings for this.

## Effects

This interactions cancels any pending [LIST](list.md) on the NFT with this ID. It is equivalent to
having called LIST with a cancel on it. It also cancels any pending SEND, and immediately removes
this NFT from the list of a parent NFT's children, if any.

## Standards Per Implementation

[Kusama BURN](../../kusama/interactions/burn.md)
