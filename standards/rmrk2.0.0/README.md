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

- [x] [COLLECTION + Metadata](entities/collection.md)
- [ ] [NFT + Metadata](entities/nft.md)
- [x] [BASE](entities/base.md)

The following **interactions** are possible:

- [x] [CREATE](interactions/create.md) (Minting a class of NFTs)
- [x] [CHANGEISSUER](interactions/changeissuer.md) (Changing the issuer of a collection or base)
- [x] [MINT](interactions/mint.md) (Minting an NFT inside a collection)
- [ ] [SEND](interactions/send.md) (Sending an NFT to a recipient)
- [ ] [LIST](interactions/list.md) (List an NFT for sale)
- [ ] [BUY](interactions/buy.md) (Buy an NFT)
- [x] [CONSUME](interactions/consume.md) (Burn an NFT)
- [x] [EMOTE](interactions/emote.md) (Send a reaction/emoticon)
- [ ] [EQUIP](interactions/equip.md) (Equip a child NFT into a parent's slot)
- [ ] [UNEQUIP](interactions/unequip.md) (Unequip (empty) a slot of a base)
- [ ] [SETATTRIBUTE](interactions/setattribute.md) (Set a custom value on an NFT)
- [ ] [RESADD](interactions/resadd.md) (Add a new resource to an NFT as the collection's issuer)
- [ ] [RESACCEPT](interactions/resaccept.md) (Accept the addition of a new resource to an existing NFT
  as an owner)
- [x] [BASE](interactions/base.md) (Create a [Base](entities/base.md))
- [x] [EQUIPPABLE](interactions/equippable.md) (Changes the list of equippable collections on a base's part)
