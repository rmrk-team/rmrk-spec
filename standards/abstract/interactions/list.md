# LIST interaction (Abstract)

A LIST interaction lists an NFT as available for sale. The NFT can be instantly purchased. A listing
can be canceled, and is automatically considered canceled when a [BUY](./buy.md) is executed on top
of a given LIST.

You can only LIST an existing NFT (one that has not been [BURNed](burn.md) yet).

An NFT that has another NFT as its owner CAN NOT be LISTed. An NFT-owned NFT must first be sent to
an account before being LISTed.

Selling an NFT which owns other NFTs will also transfer the ownership of those child NFTs. This is
bundling logic at play, allowing the trade of fully equipped game characters or even just simple
bundles (sets) of NFTs (e.g. Kanaria armor set and similar).

## Standard
- `id`: NFT ID
- `price` listing price of the item, expressed in plancks (lowest denomination available on
  chain), e.g. `10000` for `1 KSM`. Alternatively, `price` can also be `0` which cancels the
  listing.

## Effects

An NFT's LIST is canceled by setting the price to `0`, or by the NFT being [sold](buy.md),
[sent](send.md), or [burned](burn.md). While these actions will **NOT** emit the appropriate LIST
cancel interaction, they are **IMPLIED** to cancel a listing.

Note: submitting a new LIST with a different price changes the listing price.

## Standards Per Implementation

[Kusama LIST](../../kusama/interactions/list.md)
