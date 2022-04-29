# SEND interaction (Abstract)

Send an [NFT](../entities/nft.md) to an arbitrary recipient.

You can only SEND an existing NFT (one that has not been [BURNed](burn.md) yet).

When sending an NFT to another NFT, unless you own both, the SEND has to be [ACCEPT](accept.md)ed.

## Standard
- `id`: [nft](../entity/nft.md)'s ID
- `recipient`: recipient, either Account or NFT ID

## Effects

This interactions cancels any pending [LIST](list.md) on the NFT with this ID. It is equivalent to
having called LIST with a cancel on it. It also cancels any pending SENDs when doing NFT to NFT
transfer.

## Standards Per Implementation

[Kusama SEND](../../kusama/interactions/send.md)

[Substrate SEND](../../substrate/interactions/send.md)

[EVM SEND](../../evm/interactions/send.md)