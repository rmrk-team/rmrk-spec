# RMRK.app Specification and Standards

This repository hosts standards for creating and interacting with [RMRK.app](https://rmrk.app) NFTs.

There are two types of standards: _entities_ and _interactions_. Entities are definitions for how a
collection or NFT should be declared while being minted, so that tools can recognize them as valid.
Interactions are interactions between the off-chain world and the RMRK protocol, between those NFTs,
and between different users and NFTs.

## Interactions

The standards currently cover the following basic interactions:

- MINT creates a new collection
- MINTNFT creates a new NFT inside an existing collection
- CONSUME burns an NFT
- CHANGEISSUER changes the owner of the collection, affecting who can issue NFTs
- SEND, BUY are transactional
- LIST is used to list an NFT as for-sale on-chain
- MIGRATE (starting with 1.0.0) allows a collection issuer to migrate a collection and its child
  NFTs to a new standard

Inspect each standard folder to see the specifics about these interactions and how to integrate
them.

## Entities

A collection of NFTs is an entity. An NFT inside a collection is an entity as well.

## Releases

RMRK standards versions will be tagged as a [release](/Swader/rmrk-spec/releases). Each release will
contain all previous major and minor versions so that an implementer checking out the latest release
can decide which ones to support alongside the latest.

A released version is **never** worked on again - all changes must happen via [RIPs](#contributing)
and will apply to the next version only. It is up to the implementing library / UI to see previous
versions as invalid or demand a migration from an old version to a new version of an NFT (this will
usually involve a [CONSUME](#interactions) of an earlier NFT and a [MINTNFT](#interactions) into a
newer version of the collection) or a [MIGRATE](#interactions) interaction. To be kept up to date on
version changes, please _Watch_ this repo or [subscribe to Dot Leap](https://dotleap.substack.com)
or [NFT Review](https://news.nft.review).

### Currently published standards:

- [0.1](https://github.com/Swader/rmrk-spec/releases/tag/0.1)
- [1.0.0](https://github.com/Swader/rmrk-spec/releases/tag/1.0.0)

### Standards under development:

- n/a

## Contributing

To contribute, please create a new RIP (RMRK Improvement Proposal) as per
[this template](https://github.com/Swader/rmrk-spec/issues/new?assignees=&labels=RIP&template=rip.md&title=RIP-XXX+%28please+change+XXX+to+number%29).

## Implementations

Interested in implementing these standards? Check out the
[Implementer's Guide](implementers-guide.md).

The following is a list of implementations of these standards:

- [RMRK.app Tools (TypeScript)](https://github.com/swader/rmrk-tools) (IN DEVELOPMENT)

## Extending the Standard

The RMRK standards will always support the very minimum of functionality needed to interact with
NFTs. However, some projects may want or need more logic, or more specific integration. Let's say
for example that someone is building a CryptoKitties game in which cats can "breed" to produce an
offspring with some random on-chain attributes adding special uniqueness to the new cat token. The
game devs would:

- integrate with an [implementation](#implementations) of RMRK
- wrap RMRK functionality in their own library
- define their own game's rules
- MINT and do other [interactions](#interactions) as per RMRK standards, but reacting to their
  custom rules

As an example, let's assume that we have a user who owns Cat A and Cat B and wants to breed them to
get Cat C. Let's assume that breeding requires 100 blocks of time, and that a breeding results in an
egg, this egg needs to incubate for 100 blocks, and then the user needs to manually hatch it.

To implement this, the CryptoKitties game devs would:

- add a BREED and a HATCH interaction on top of the RMRK standards they already inherit through an
  [implementation](#implementations).
- once BREED is called by a user, the starting block is logged (e.g. X).
- at block X+100, the user can call HATCH and the egg hatches. The block hash is used as a seed for
  the random attributes of the new NFT.
- a new NFT is created and assigned to the breeder.
- given that the NFT is minted according to RMRK standards, it will be automatically compatible with
  any tools and UIs that follow the standard as well.

> Note: A tricky part is step 4 - minting and assigning a new NFT to the breeder. This would require
> a new entity type given that only an issuer is allowed to mint new NFTs, but then the new entity
> is not automatically compatible with wallets and UIs following RMRK, given that they're not
> familiar with the CryptoKitties standard. This introduces a barrier to market entry for the
> CryptoKitties devs. A better alternative is skipping user-initiated hatching and hatching
> programmatically from CryptoKitties game devs' end, but this introduces a reliance on their
> service for the continuation of the game.

> This is an active area of research which we hope to have ironed out soon - ideally with an
> action-triggered MINT addition to the standard.

## Other Information

- [The definitive primer on NFTs](https://bitfalls.com/nft)
- [The RMRK.app website](https://rmrk.app)
