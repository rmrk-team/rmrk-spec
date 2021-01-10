## RMRK 0.1.1. 09.01.2021.

### RIPs

- [RIP #001](https://github.com/Swader/rmrk-spec/issues/2): the NFT entity's computed ID field has
  been updated to include minting block number.
- [RIP #004](https://github.com/Swader/rmrk-spec/issues/5): MIGRATE interaction for migrating
  collections to newer standards.

### Fixes

- Changed `batch` to much safer `batchAll` across the standard.
- Fixed some dead links.
- Standard versions have been added to MINT and MINTNFT where previously there were none. Tools
  should consider these interactions as 0.1 where non-specified.
