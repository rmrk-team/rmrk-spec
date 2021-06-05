# EMOTE

React to an on-chain entity with an emoticon.

The EMOTE interaction allows you to emote on:

- any non-[consume](consume.md)d NFT
- any Substrate account

## Standard

The format of a EMOTE interaction is
`0x{bytes(rmrk::EMOTE::{version}::{namespace}::{id}::{emotion})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `namespace` is the namespace to which the ID belongs.
- `id` is the ID of the entity. See namespace table below.
- `emotion` is the unicode of the emoji (e.g Unicode for party popper 🎉 is
  [1F389](https://emojipedia.org/emoji/🎉/).

| Namespace | ID references                                                                                     |
| --------- | ------------------------------------------------------------------------------------------------- |
| rmrk1     | [nft](../entity/nft.md)'s ID [computed field](../../rmrk/1.0.0/entities/nft.md/#computed-fields). |
| rmrk2     | [nft](../entity/nft.md)'s ID [computed field](../entities/nft.md/#computed-fields).               |
| pubkey    | Public key of an account, allows emoting on people's accounts.                                    |
| ink       | Unique identifier of smart contract.                                                              |
| eth       | Hex address of EVM smart contract or Ethereum EOA.                                                |

### Undo Emote

A user can undo their emote by sending the same emote again. Thus, during consolidation of RMRK
states:

- a tool should consider an emote of type E on NFT N by user U to be absent if `n(U:E,N)%2==0`
- a tool should consider an emote of type E on NFT N by user U to be present if `n(U:E,N)%2!=0`

In other words, if there is an even number of emotes on an NFT by a single user, that user has NOT
emoted. If there is an odd number, that user HAS emoted.

Example:

- Alice gives thumbs up to `rmrk1::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001`. Now there
  is one 👍 emote on this NFT, shown as 👍 from Alice.
- Alice changes her mind, removes thumbs up by sending it again. Now there are 2(👍) on this NFT, no
  👍 is shown on the NFT from Alice.
- Alice talks to artist, she's okay with a thumbs up again, re-sends it. Now there are 3(👍) on this
  NFT, shown as just 👍 from Alice.

### Multiple Emotes

It is perfectly valid for the same user to send multiple emotes to the same NFT. The same rules
about Undoing apply individually to each emote.

### Supported Emotes

There is no limitation to which emotes can be sent - any unicode works. However, it is up to the
individual UIs to decide which ones to show. For example, one UI might be so positivity-oriented
they decide to never show 💩. Others might display some emoji which only exist on certain operating
systems. We leave the choice to the implementers and direct them to emojipedia's stats for most used
emoji: https://emojipedia.org/stats/

## Examples

### Emoting on an NFT

```
rmrk::EMOTE::2.0.0::rmrk1::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001::1F389
```

Is submitted as:

```
0x726d726b3a3a454d4f54453a3a322e302e303a3a726d726b313a3a353130353030302d306166663638363562656433613636622d444c45502d444c31352d303030303030303030303030303030313a3a3146333839
```

### Emoting on an Account

```
rmrk::EMOTE::2.0.0::pubkey::0xe81f67c2def10f4cc1f43b0e207921210ff83747eb354ad653bbd2c0f0466f10::1F389
```

Is submitted as:

```
0x726d726b3a3a454d4f54453a3a322e302e303a3a7075626b65793a3a3078653831663637633264656631306634636331663433623065323037393231323130666638333734376562333534616436353362626432633066303436366631303a3a31463338390a
```

## Implementations

- Smart Contract (!ink) - TBD
- Smart Contract (EVM) - TBD
- [Pallet](https://github.com/rmrk-team/pallet-emotes)
- RMRK Tools (Typescript) - TBD
