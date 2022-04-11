# Kusama ACCEPT interaction

## Standard

The Kusama format of an [ACCEPT](../../abstract/interactions/accept.md) interaction is `0x{bytes(rmrk::ACCEPT::{version}::{id1}::{entity}::{id2})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id1` is the [NFT](../abstract/entities/nft.md)'s ID
- `entity` is the type of entity being accepted: `RES` or `NFT`.
- `id2` is the id of the entity being accepted

## Example for Resources

Suppose we have an NFT with the ID `5105000-0aff6865bed3a66b-DLEP-DL15-00000001`.

Suppose it has the following resource pending:

```json
{
  "id": "V1i6B",
  "src": "hash-of-metadata-guest-bird-art-with-jetpack",
  "metadata": "hash-of-metadata-with-credits"
}
```

Via:

```txt
rmrk::RESADD::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::%7B%22id%22%3A%22V1i6B%22%2C%src%22%3A%22hash-of-metadata-guest-bird-art-with-jetpack%22%2C%22metadata%22%3A%22hash-of-metadata-with-credits%22%7D
```

We can accept it with:

```txt
rmrk::ACCEPT::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::RES::V1i6B
```

After acceptance, the NFT changes from:

```json
{
  "collection": "0aff6865bed3a66b-DLEP",
  "symbol": "DL15",
  "transferable": 1,
  "sn": "00000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4"
}
```

to

```json
{
  "collection": "0aff6865bed3a66b-DLEP",
  "symbol": "DL15",
  "transferable": 1,
  "sn": "00000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4",
  "priority": [0],
  "resources": [
    {
      "id": "V1i6B",
      "src": "hash-of-metadata-guest-bird-art-with-jetpack",
      "metadata": "hash-of-metadata-with-credits"
    }
  ]
}
```

## Example for Children

Suppose we have an NFT with the ID `5105000-0aff6865bed3a66b-DLEP-DL15-00000001`.

Suppose its `children` property looks like this:

```json
"children": [];
```

Suppose we want to `SEND` it the NFT with an ID `5105000-0aff6865bed3a66b-DLEP-DL15-00000002`.

We would first issue:

```
RMRK::SEND::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000002::5105000-0aff6865bed3a66b-DLEP-DL15-00000001
```

If we own both NFTs, this concludes the interaction.

If we do not, we have this:

```json
"children": [
  {
    "id": "5105000-0aff6865bed3a66b-DLEP-DL15-00000002",
    "equipped": "",
    "pending": true,
  },
]
```

So, we must issue an `ACCEPT` like so:

```
rmrk::ACCEPT::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::NFT::5105000-0aff6865bed3a66b-DLEP-DL15-00000002
```

Thereafter, the children array will be:

```json
"children": [
  {
    "id": "5105000-0aff6865bed3a66b-DLEP-DL15-00000002",
    "equipped": "",
    "pending": false,
  },
]
```
