# BUY interaction (Abstract)

The BUY interaction allows a user to immediately purchase an [NFT](../entities/nft.md) listed for
sale using the [LIST](list.md) interaction, as long as the listing hasn't been canceled.

You can only BUY an existing NFT (one that has not been [BURNed](burn.md) yet). You can only BUY a
root-level NFT, meaning one owned by an account, and not owned by another NFT. You can buy an NFT
which contains other NFTs and they all get transfered with it.

## Effects

This interactions cancels any pending [LIST](list.md) on the NFT with this ID. It is equivalent to
having called LIST with a cancel on it.

## Standards Per Implementation

[Kusama BUY](../../kusama/interactions/buy.md)
