## RMRK v1.0.0 - 09.01.2021.

### RIPs

- [RIP #001](https://github.com/Swader/rmrk-spec/issues/2): the NFT entity's computed ID field has
  been updated to include minting block number.
- [RIP #002](https://github.com/Swader/rmrk-spec/issues/3): Embedded data - ability to directly add
  data to an NFT and a collection, avoiding reliance on third party metadata. Accompanying metadata
  is still recommended.
- [RIP #004](https://github.com/Swader/rmrk-spec/issues/5): MIGRATE interaction for migrating
  collections to newer standards.
- [RIP #005](https://github.com/Swader/rmrk-spec/issues/6): Changing recommendation to use
  `batchAll` instead of `batch`

### Fixes

- Changed `batch` to much safer `batchAll` across the standard.
- Fixed some dead links.
- Standard versions have been added to MINT and MINTNFT where previously there were none. Tools
  should consider these interactions as 0.1 where non-specified.
