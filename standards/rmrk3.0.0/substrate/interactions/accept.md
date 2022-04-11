## Standard

The format of an [ACCEPT](../abstract/interactions/accept.md) interaction with Substrate is `...`.

## Example for ACCEPTing a Pending Resource

Suppose we have an NFT with the ID `5105000-0aff6865bed3a66b-DLEP-DL15-00000001`.

Suppose it has the following resource pending:

```
TODO example of EVM pending resource
```

We can accept it with:

```txt
TODO how to ACCEPT with EVM
```

After acceptance, the NFT changes from:

```
TODO NFT before change in EVM
```

to

```
TODO NFT after change
```

## Example for ACCEPTing a Pending Child

Suppose we have an NFT with the ID `5105000-0aff6865bed3a66b-DLEP-DL15-00000001`.

Suppose its `children` looks like this:

```
TODO EVM empty children
```

Suppose we want to `SEND` it the NFT with an ID `5105000-0aff6865bed3a66b-DLEP-DL15-00000002`.

We would first issue:

```
TODO EVM SEND operation
```

If we own both NFTs, this concludes the interaction.

If we do not, we have this:

```
TODO EVM Pending Child
```

So, we must issue an `ACCEPT` like so:

```
TODO EVM ACCEPT for pending NFT
```

Thereafter, the children array will be:

```
TODO EVM post-child-ACCEPT
```
