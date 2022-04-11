# DESTROY

The DESTROY interaction destroys a [Collection](../entities/collection.md).

You can only DESTROY an existing Collection (one that has not been destroyed yet), only if you are it's issuer and as long as it doesn't contain any unburned NFTs.

## Standard

The format of DESTROY interaction is: `0x{bytes(rmrk::DESTROY::{version}::{id})}`

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [collection](../entities/collection.md)'s ID.

## Example

`RMRK::DESTROY::2.0.0::0a24c7876a892acb79-PUNKS`
