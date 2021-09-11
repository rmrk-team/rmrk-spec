# SETPRIORITY

The `SETPRIORITY` interaction allows NFT owners to change the priority (rendering order) or their
[resources](../entities/nft.md#resources-and-resource).

## Format

```txt
rmrk::SETPRIORITY::2.0.0::{id}::{html_encoded_value}
```

- `html_encoded_value` is the full array of resource IDs. It is **always** a comma-separated string
  of IDs.

Example. Given an NFT `5105000-0aff6865bed3a66b-DLEP-DL15-00000001` with resources `foo`, `bar`, and
`baz`, if we want to make `bar` the priority resource, and `baz` the least important, we issue the
following:

```txt
rmrk::SETPRIORITY::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-00000001::bar,foo,baz
```

You **can** set priority with a resource ID that does not reference an existing resource. Invalid
resource IDs will simply be ignored.
