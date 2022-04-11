# EQUIPPABLE

The EQUIPPABLE interaction allows a [Base](../entities/base.md) owner to modify the list of
equippable collections on a Base's slot.

A Base starts with an empty string `""` as a value for `equippable` on each of its slots.

## Standard

The format of a EQUIPPABLE interaction is
`0x{bytes(rmrk::EQUIPPABLE::{version}::{id}::{slot}::{+/-?collections|*})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [base](../entities/base.md)'s computed ID.
- `slot` is the id of the slot, e.g. `wing_slot_1`
- `+/-?collections|*` is the value. If a string or set of comma-separated strings is prefixed with a
  `+`, those collections are added. If the prefix is `-`, they are removed. If there is no prefix,
  the field is overwritten in full. If the value is `*`, this means "all", making all collections
  equippable into this slot.

## Example

If we want to change the permission of Base `base-575878273-kanaria_epic_birds` to allow anyone to
mint any item for holding in the bird's left wing, we would do:

```bash
rmrk::EQUIPPABLE::2.0.0::base-575878273-kanaria_epic_birds::wing_slot_1::*
```

This makes the property look like this:

```json
"equippable": ["*"]
```

If we want to revert this change later and make it so that only items from collection
`0aff6865bed3a66b-DLEP` can be equipped, we issue:

```bash
rmrk::EQUIPPABLE::2.0.0::base-575878273-kanaria_epic_birds::wing_slot_1::0aff6865bed3a66b-DLEP
```

This overrides the `*` from before because there is no `+`/`-` prefix.

If we later want to add another collection into the mix, we issue:

```bash
rmrk::EQUIPPABLE::2.0.0::base-575878273-kanaria_epic_birds::wing_slot_1::+0aff6865bed3a66b-FOO
```

When consolidated, the final result of the `equippable` field of this base is:

```bash
"equippable": ["0aff6865bed3a66b-DLEP", "0aff6865bed3a66b-FOO"]
```

When all collections are removed from a BASE's equippable list, the value becomes an empty string
`""`.
