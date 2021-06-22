# BASEOWNER

The BASEOWNER interaction allows a [Base](../entities/base.md) owner to change the owner field to
another address. The original owner immediately loses all rights to further modify the base.

This is particularly useful when another team is taking control over a project, or if an address was
compromised.

## Standard

The format of a BASEOWNER interaction is `0x{bytes(rmrk::BASEOWNER::{version}::{id}::{newissuer})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [base](../entities/base.md)'s ID.
- `newowner` is the address of the new owner, e.g. `CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp`

## Example

If we want to change the owner of Base `kanaria-epic-birds` to
`HviHUSkM5SknXzYuPCSfst3CXK4Yg6SWeroP6TdTZBZJbVT`, we would compose:

```
rmrk::BASEOWNER::2.0.0::kanaria-epic-birds::HviHUSkM5SknXzYuPCSfst3CXK4Yg6SWeroP6TdTZBZJbVT
```

Which is submitted as:

```
0x726d726b3a3a424153454f574e45523a3a322e302e303a3a6b616e617269612d657069632d62697264733a3a4876694855536b4d35536b6e587a59755043536673743343584b34596736535765726f50365464545a425a4a625654
```
