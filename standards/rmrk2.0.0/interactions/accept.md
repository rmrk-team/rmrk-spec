# ACCEPT

The ACCEPT interaction allows the owner of an [NFT](../entities/nft.md) to accept a pending resource
added by [RESADD](resadd.md), or to accept a child NFT that is being [SENT](send.md) into a parent
NFT.

## Implications for Resources

If the resource being accepted is the first resource of this NFT, then this operations results in
the NFT being given a `priority` property with the value of `[0]`, meaning the first resource of
this NFT is highest priority.

## Pending

A resource that is added to an NFT via `RESADD` enters a pending state and MUST be accepted with an
`ACCEPT` unless the owner of the NFT is the same account as the issuer of the collection, in which
case it is automatically accepted even without a `ACCEPT` ever being issued.

A child NFT that is sent to another NFT via `SEND` enters a pending state and MUST be accepted with
an `ACCEPT` unless the `rootowner` of both NFTs matches.

## Removal of resources

It is not possible to remove resources accepted in the past. This prevents art rug-pulls.

## Standard for resources

The format of an ACCEPT interaction is `0x{bytes(rmrk::ACCEPT::{version}::{id}::{md5})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [NFT](../entities/nft.md)'s ID
- `md5` is the md5 hash of the Resource being accepted

## Standard for child NFTs

The format of an ACCEPT interaction is `0x{bytes(rmrk::ACCEPT::{version}::{id}::{childId})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [NFT](../entities/nft.md)'s ID
- `childId` is the ID of the pending child

## Example for Resources

Suppose we have an NFT with the ID `5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001`.

Suppose it has the following resource pending:

```json
{
  "media": "hash-of-metadata-guest-bird-art-with-jetpack",
  "metadata": "hash-of-metadata-with-credits"
}
```

Via:

```txt
rmrk::RESADD::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001::%7B%22media%22%3A%22hash-of-metadata-guest-bird-art-with-jetpack%22%2C%22metadata%22%3A%22hash-of-metadata-with-credits%22%7D
```

We can accept it with:

```txt
rmrk::ACCEPT::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001::5dd473899a96cec2c688ed118bb4da75
```

After acceptance, the NFT changes from:

```json
{
  "nftclass": "0aff6865bed3a66b-DLEP",
  "symbol": "DL15",
  "transferable": 1,
  "sn": "0000000000000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4"
}
```

to

```json
{
  "nftclass": "0aff6865bed3a66b-DLEP",
  "symbol": "DL15",
  "transferable": 1,
  "sn": "0000000000000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4",
  "priority": [0],
  "resources": [
    {
      "media": "hash-of-metadata-guest-bird-art-with-jetpack",
      "metadata": "hash-of-metadata-with-credits"
    }
  ]
}
```

## Example for Children

Suppose we have an NFT with the ID `5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001`.

Suppose its `children` property looks like this:

```js
children: {
}
```

Suppose we want to `SEND` it the NFT with an ID
`5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000002`.

We would first issue:

```
RMRK::SEND::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000002::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001
```

If we own both NFTs, this concludes the interaction. If we do not, we must issue an `ACCEPT` like
so:

```
rmrk::ACCEPT::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000002
```

Thereafter, the children object will be:

```js
children: {
  "5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000002": ""
}
```
