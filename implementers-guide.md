# RMRK Implementer's Guide

Because RMRK is append-only and no state is being changed, every new block has the potential to
introduce non-obvious state changes into the global NFT space living on RMRK. As an example, an
[NFT](standards/rmrk0.1/entities/nft.md) can change ownership due to a
[SEND](standards/rmrk0.1/interactions/send.md) interaction, or due to a
[BUY](standards/rmrk0.1/interactions/send.md) interaction. It is an implementer's duty to reconcile
all these changes across historical blocks to get an accurate picture of the current state.

> Note: By implementing things in the wrong order or not following recommended implementation
> guidelines, an implementer can cause financial loss to NFT minters, buyers, and sellers. There is
> no way to distinguish invalid interaction submissions compatible with RMRK from those that are
> valid, except by reconciling the **whole RMRK history from genesis**.

---

This guide is still under development...
