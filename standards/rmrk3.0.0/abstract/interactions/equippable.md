# EQUIPPABLE interaction (Abstract)

The EQUIPPABLE interaction allows a [Base](../entities/base.md) owner to modify the list of
equippable collections on a Base's slot.

A Base starts with nothing/none as a value for `equippable` on each of its slots.

## Standard
- id: Base ID
- slot: Slot ID
- value: Must be able to (1) set to one or more Collection IDs, (2) add one or more Collection IDs, (3) remove one or more Collection IDs, (4) set to a generic "All" Collection IDs

## Standards Per Implementation

[Kusama EQUIPPABLE](../../kusama/interactions/equippable.md)

[Substrate EQUIPPABLE](../../substrate/interactions/equippable.md)

[EVM EQUIPPABLE](../../evm/interactions/equippable.md)