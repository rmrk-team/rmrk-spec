# RMRK 2.0.0 standard

RMRK 2.0 is a significant departure from 1.0. in that it brings into being the following features:

- NFTs owning NFTs
- NFTs governed as DAOs via shareholder tokens (issue commands to NFTs democratically) (coming when
  we transition to Unique)
- NFTs having multiple priority-ordered client-dependent resources (a book with a cover and an audio
  version)
- NFTs, accounts, and other on-chain entities being emoted to (on-chain emotes), allowing early
  price discovery without listing, selling, bidding.
- NFTs with conditional rendering (on-NFT logic allowing different visuals and resources to trigger
  depending on on-chain and off-chain conditions).

The following **entities** are defined:

- [x] [Collection + Metadata](entities/collection.md)
- [x] [NFT + Metadata](entities/nft.md)
- [x] [BASE](entities/base.md) - template resource with themes and parts from which to build an
      NFT's render

The following **interactions** are possible:

- [x] [ACCEPT](interactions/accept.md) (Accept the addition of a new resource to an existing NFT, or
      the additiona of a child into a parent NFT)
- [x] [BASE](interactions/base.md) (Create a [Base](entities/base.md))
- [x] [BUY](interactions/buy.md) (Buy an NFT)
- [x] [CHANGEISSUER](interactions/changeissuer.md) (Changing the issuer of a collection or base)
- [x] [CONSUME](interactions/consume.md) (Burn an NFT)
- [x] [CREATE](interactions/create.md) (Minting a collection of NFTs)
- [x] [EMOTE](interactions/emote.md) (Send a reaction/emoticon)
- [x] [EQUIP](interactions/equip.md) (Equip a child NFT into a parent's slot, or unequip)
- [x] [EQUIPPABLE](interactions/equippable.md) (Changes the list of equippable collections on a
      base's part)
- [x] [LIST](interactions/list.md) (List an NFT for sale)
- [x] [LOCK](interactions/lock.md) (Locking a collection)
- [x] [MINT](interactions/mint.md) (Minting an NFT inside a collection)
- [x] [RESADD](interactions/resadd.md) (Add a new resource to an NFT as the collection issuer)
- [x] [SEND](interactions/send.md) (Sending an NFT to a recipient)
- [x] [SETATTRIBUTE](interactions/setattribute.md) (Set a custom value on an NFT)
- [x] [SETPRIORITY](interactions/setpriority.md) (Set a different order of resource priority)
- [x] [THEMEADD](interactions/themeadd.md) (Add a new theme to a base)
