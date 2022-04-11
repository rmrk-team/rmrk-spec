# CHANGEISSUER interaction (Abstract)

The CHANGEISSUER interaction allows a [Base](../entities/base.md) or
[Collection](../entities/collection.md) issuer to change the issuer value to another address. The
original issuer immediately loses all rights to further interact with the entity.

This is particularly useful when another team is taking control over a project, or if an address was
compromised.

## Standards Per Implementation

[Kusama ACCEPT](../../kusama/interactions/accept.md)

[Substrate ACCEPT](../../substrate/interactions/accept.md)

[EVM ACCEPT](../../evm/interactions/accept.md)