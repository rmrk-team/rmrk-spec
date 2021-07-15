# EMOTE

React to an on-chain entity with an emoticon.

The EMOTE interaction on Kusama allows you to emote on:

- any non-[consume](consume.md)d NFT
- any Substrate account

Outside of Kusama, the emote interaction can also be used on other entities (see namespace tables
below).

## Standard

The format of a EMOTE interaction is
`0x{bytes(RMRK::EMOTE::{version}::{namespace}::{id}::{emotion})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `namespace` is the namespace to which the ID belongs.
- `id` is the ID of the entity. See namespace table below.
- `emotion` is the unicode of the emoji (e.g Unicode for party popper 🎉 is
  [1F389](https://emojipedia.org/emoji/🎉/).

| Namespace | ID references                                                                                                     |
| --------- | ----------------------------------------------------------------------------------------------------------------- |
| RMRK1     | [nft](../../rmrk/1.0.0/entities/nft.md)'s ID [computed field](../../rmrk/1.0.0/entities/nft.md/#computed-fields). |
| RMRK2     | [nft](../entities/nft.md)'s ID [computed field](../entities/nft.md/#computed-fields).                             |
| PUBKEY    | Public key of an account, allows emoting on people's accounts.                                                    |

Uppercase namespaces are a RMRK convention and do not need to be followed religiously. Other
implementations, depending on the chain, can add their own namespace like:

| Namespace | ID references                                      |
| --------- | -------------------------------------------------- |
| ink       | Unique identifier of smart contract.               |
| eth       | Hex address of EVM smart contract or Ethereum EOA. |
| sub:space | Subsocial space                                    |
| sub:post  | Subsocial post                                     |

### Undo Emote

A user can undo their emote by sending the same emote again. Thus, during consolidation of RMRK
states:

- a tool should consider an emote of type E on NFT N by user U to be absent if `n(U:E,N)%2==0`
- a tool should consider an emote of type E on NFT N by user U to be present if `n(U:E,N)%2!=0`

In other words, if there is an even number of emotes on an NFT by a single user, that user has NOT
emoted. If there is an odd number, that user HAS emoted.

Example:

- Alice gives thumbs up to `RMRK1::5105000-0aff6865bed3a66b-DLEP-DL15-00000001`. Now there is one 👍
  emote on this NFT, shown as 👍 from Alice.
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

When validating emoji inputs, the standard set supported by tools such as
[this one](https://github.com/mathiasbynens/emoji-regex) should be used, and nothing beyond it. This
is so the emote functionality does not end up being abused for things like message-passing as there
are separate implementations for this.

## Examples

### Emoting on an NFT

```
RMRK::EMOTE::2.0.0::RMRK1::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::1F389
```

### Emoting on an Account

```
RMRK::EMOTE::2.0.0::PUBKEY::0xe81f67c2def10f4cc1f43b0e207921210ff83747eb354ad653bbd2c0f0466f10::1F389
```

## Implementations

- Smart Contract (!ink) - TBD
- Smart Contract (EVM) - TBD
- [Pallet](https://github.com/rmrk-team/pallet-emotes)
- RMRK Tools (Typescript) - TBD

## Useful tools

- [Emoji Regex](https://github.com/mathiasbynens/emoji-regex) (Typescript / JavaScript)
