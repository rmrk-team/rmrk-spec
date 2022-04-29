# EQUIP interaction (Abstract)

Equips an owned [NFT](../entities/nft.md) into a slot on its parent, or unequips it.

You can only EQUIP an existing NFT (one that has not been [BURNed](burn.md) yet). You can only EQUIP
an NFT into its immediate parent. You cannot equip across ancestors, or even across other NFTs. You
can only unequip an equipped NFT.

You can equip/unequip a non-transferable NFT. As an example, putting a helmet on or taking it off
does not change the ownership of the helmet.

You can only equip a [non-pending](accept.md) child NFT.

### Baseslot Explained

Parts on a [base](entities/base.md) and resources on an [nft](entities/nft.md) reference a property
called `slot`.

Suppose we have an NFT A with resource `base_a` which contains slot `slot_1`. Now suppose we
[RESADD](interactions/resadd.md) a new resource, `base_b`, onto this NFT. Suppose that `base_b` also
has a slot called `slot_1`. We now have NFT A with two resources, each of which has `slot_1`.
Suppose now that we have an NFT which has a resource that can equip into `slot_1`.

If we now [SEND](interactions/send.md) this NFT into NFT A and issue the
[EQUIP](interactions/equip.md) command, the renderer would not know which slot to put the NFT's
resource into as a layer: `base_b` or `base_a`. Therefore, a `base` and `slot` namespaced
combination is necessary.

## Standards Per Implementation

[Kusama EQUIP](../../kusama/interactions/equip.md)

[Substrate EQUIP](../../substrate/interactions/equip.md)

[EVM EQUIP](../../evm/interactions/equip.md)