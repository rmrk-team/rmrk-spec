# MIGRATE

The MIGRATE interaction migrates an [NFT collection](../entities/collection.md) to this standard
from a previous version.

MIGRATE can only be called by the current issuer of a collection.

## Standard

The format of a MIGRATE interaction is
`0x{bytes(rmrk::MIGRATE::{version}::{id}::{html_encoded_changes})}`.

- `version` is the version of the current standard (e.g. `1.0.0`)
- `id` is the [collection](../entity/collection.md)'s ID.
- `html_encoded_changes`: each migration has a specific set of requirements for moving from one
  version to another. Some might drop fields, others might get them. Some fields might even update
  to reflect changes in the standard. These changes are to be defined in the standard of a
  migration. At the very least, a migration will include a version number bump.

### Changes for migrating from 0.1 to 1.0.0

```json
{
  "version": "1.0.0"
}
```

The object should be minified before being HTML encoded.

## Examples

Provided we have:

```json
{
  "version": "0.1",
  "name": "Dot Leap Early Promoters",
  "max": 100,
  "issuer": "CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp",
  "symbol": "DLEP",
  "id": "0aff6865bed3a66b-DLEP",
  "metadata": "ipfs://ipfs/QmVgs8P4awhZpFXhkkgnCwBp4AdKRj3F9K58mCZ6fxvn3j"
}
```

And we want to upgrade it to 1.0.0:

```
rmrk::MIGRATE::1.0.0::0aff6865bed3a66b-DLEP::%7B%22version%22%3A+%221.0.0%22%7D
```

In bytes:

```
0x726d726b3a3a4d4947524154453a3a312e302e303a3a306166663638363562656433613636622d444c45503a3a25374225323276657273696f6e2532322533412b253232312e302e302532322537440a
```

The result, when parsed by consolidator tools, should be:

```json
{
  "version": "1.0.0",
  "name": "Dot Leap Early Promoters",
  "max": 100,
  "issuer": "CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp",
  "symbol": "DLEP",
  "id": "0aff6865bed3a66b-DLEP",
  "metadata": "ipfs://ipfs/QmVgs8P4awhZpFXhkkgnCwBp4AdKRj3F9K58mCZ6fxvn3j"
}
```
