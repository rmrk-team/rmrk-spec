# RMRK Implementer's Guide

Because RMRK is append-only and no state is being changed, every new block has the potential to
introduce non-obvious state changes into the global NFT space living on RMRK. As an example, an
[NFT](standards/rmrk0.1/entities/nft.md) can change ownership due to a
[SEND](standards/rmrk0.1/interactions/send.md) interaction, or due to a
[BUY](standards/rmrk0.1/interactions/buy.md) interaction. It is an implementer's duty to reconcile
all these changes across historical blocks to get an accurate picture of the current state.

> Note: By implementing things in the wrong order or not following recommended implementation
> guidelines, an implementer can cause financial loss to NFT minters, buyers, and sellers. There is
> no way to distinguish invalid interaction submissions compatible with RMRK from those that are
> valid, except by reconciling the **whole RMRK history from genesis**.

## Remark Dumps

For convenience, dumps of all RMRK remarks on Kusama are stored in this repository for ease of
use. Running the `fetch` function of [rmrk-tools](https://github.com/swader/rmrk-tools) with the
appropriate block numbers will produce the same dumps, should one wish to double-check these data
sets.

## Consolidators

Consolidators are tools that take as input a list of remarks from the chain (ideally from dumps -
see above), process them one by one, and come up with a consolidated state of the NFT ecosystem on
that chain. The reference implementation is the [rmrk-tools](https://github.com/swader/rmrk-tools)
consolidator.

## Syncing

When using a server, this is easy. Load into a database and serve from there. However, to properly sync with the state in a decentralized way, an implementer must:

- grab a previously created dump of remarks until block X
- run a fetch operation to grab remaks from X to last finalized at time of page load (F)
- subscribe to new blocks since the page loaded (Y)

A possible problem occurs in cases where F < Y. Finalized blocks may lag a bit, and by default we only consolidate finalized blocks to be sure a tx went through.

To help implementers get around this problem, a [Listener tool](https://github.com/Swader/rmrk-tools) exists in the official tools repo.
## Edge Cases and Caveats

### Missing version

Spec 0.1 had a bug - the version number of the standard was not included in the MINT and MINTNFT
interactions. Where missing, this version should be implied to mean 0.1. Since 1.0.0 onwards every
interaction has a version number.

### Batching

- `batch` operations are illegal since version 1.0.0 and `batchAll` should be used. See
  [RIP #005](https://github.com/Swader/rmrk-spec/issues/6).
- due to double-spend issues and problematics of tracking large pools of unrelated remarks, it is important for implementers to prevent users from issuing multiple BUYs in a single batch. If a user wants to they can of course manually build such transactions, and the UI should see the first transaction in a block as valid, but UIs should absolutely prevent this.
- it is an implementer's responsibility to accurately track duplicate interactions in a single block. If a single block contains a LIST and a CONSUME, then whichever comes first in the finalized block is valid, the other is discarded. The Consolidator from the [official tools](https://github.com/Swader/rmrk-tools) should help with this.

---

This guide is still under development...
