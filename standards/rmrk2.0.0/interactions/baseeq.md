# BASEEQ

The BASEEQ interaction allows a [Base](../entities/base.md) owner to modify the list of equippable
collections on a Base's slot.

## Standard

The format of a BASEEQ interaction is
`0x{bytes(rmrk::BASEEQ::{version}::{id}::{slot}::{+/-?collections|*})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [base](../entities/base.md)'s ID.
- `slot` is the name of the slot, e.g. `wing_slot_1`
- `+/-?collections|*` is the value. If a string or set of comma-separated strings is prefixed with a
  `+`, those collections are added. If the prefix is `-`, they are removed. If there is no prefix,
  the field is overwritten in full. If the value is `*`, this means "all", making all collections
  equippable into this slot.

## Example

If we want to change the permission of Base `kanaria-epic-bird` to allow anyone to mint any item for
holding in the bird's left wing, we would do:

```bash
rmrk::BASEEQ::2.0.0::kanaria-epic-birds::wing_slot_1::*
```

Which gets submitted as

```bash
0x726d726b3a3a4241534545513a3a322e302e303a3a6b616e617269612d657069632d62697264733a3a77696e675f736c6f745f313a3a2a0a
```

If we want to revert this change later and make it so that only items from collection
`0aff6865bed3a66b-DLEP` can be equipped, we issue:

```bash
rmrk::BASEEQ::2.0.0::kanaria-epic-birds::wing_slot_1::0aff6865bed3a66b-DLEP
```

This overrides the `*` from before because there is no `+`/`-` prefix.

This is submitted as:

```bash
0x726d726b3a3a4241534545513a3a322e302e303a3a6b616e617269612d657069632d62697264733a3a77696e675f736c6f745f313a3a306166663638363562656433613636622d444c4550
```

If we later want to add another collection into the mix, we issue:

```bash
rmrk::BASEEQ::2.0.0::kanaria-epic-birds::wing_slot_1::+0aff6865bed3a66b-FOO
```

This is submitted as:

```bash
0x726d726b3a3a4241534545513a3a322e302e303a3a6b616e617269612d657069632d62697264733a3a77696e675f736c6f745f313a3a306166663638363562656433613636622d444c4550
```

When consolidated, the final result of the `equippable` field of this base is:

```bash
"equippable": ["0aff6865bed3a66b-DLEP", "0aff6865bed3a66b-FOO"]
```
