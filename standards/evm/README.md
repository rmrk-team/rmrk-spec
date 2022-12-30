# Introduction

This is the documentation of the EVM implmentation of the RMRK standard. It provides caveats and examples specific to the EVM implementation of the [Abstract](../abstract) standards.

See [RMRK](../) for documentation of other implementations.

## Specifics of EVM implementation of RMRK standard

The EVM implementation of RMRK standard refers to some of the RMRK concepts using differen vocabulary from the other implementations. This is due to the fact, that we published Ethereum Improvement proposal for each of the lego implementation and reevaluated how much information a name of the concept relays to the user. For example: `MultiResource` became `MultiAsset` which is short for `Context-Dependent Multi-Asset tokens`. The change from resource to asset stems from asset bing the best representation of what an asset of can be.

There are only a handful of differences, which can best be represented by the following spreadsheet:

| **Non-EVM term** 	| **EVM term** 	| **Long form of EVM term**                    	|
|------------------	|--------------	|----------------------------------------------	|
| MultiResource    	| MultiAsset   	| Context-Dependent Multi-Asset Tokens         	|
| Nesting          	| Nestable     	| Parent-Governed Nestable Non-Fungible Tokens 	|
| Equippable       	| Equippable   	| Composable NFTs utilizing Equippable Parts   	|
| Base             	| Catalog      	| Catalog                                      	|

<!-- TODO: Document EVM implementation -->

## Detailed specification of EVM implementation

### MultiAsset

The MultiAsset RMRK lego can be best explained with the EIP-5773 that we published. To view it, please refer to the main Ethereum's EIP directory: **[EIP-5773: Context-Dependent Multi-Asset Tokens](https://eips.ethereum.org/EIPS/eip-5773)**.

### Nestable

The Nestable RMRK lego can be best explained with the EIP-6059 that we published. To view it, please refer to the main Ethereum's EIP directory: **[EIP-6059: Parent-Governed Nestable Non-Fungible Tokens](https://eips.ethereum.org/EIPS/eip-6059)**.

### Equippable

The Equippable and Catalog RMRK legos can be best explained with the EIP-6220 that we published. To view it, please refer to the main Ethereum's EIP directory: **[EIP-6220: Composable NFTs utilizing Equippable Parts](https://eips.ethereum.org/EIPS/eip-6220)**.

### Developer documentation

The full specification of the RMRK EVM implementation is available in the **[RMRK EVM developer documentation](https://rmrk.gitbook.io/rmrk-evm-developer-documentation/rmrk-legos-examples/mergedequippable)**.

## Examples of RMRK lego EVM implementation

We have a number of implementation examples on how to use the RMRK lego EVM implementation. All of the examples use our **[@rmrk-team/evm-contracts](https://www.npmjs.com/package/@rmrk-team/evm-contracts)**

The **Sample contracts repository** contains example implementations of all of the RMRK legos and their possible combinations:

- **[Sample contracts repository](https://github.com/rmrk-team/evm-sample-contracts)**

The **MultiAsset demo repository** contains a contextualized user journey to explore and understand the RMRK MultiAsset NFT features:

- **[MultiAsset demo repository](https://github.com/rmrk-team/evm-multiasset-demo)**

The **Nestable demo repository** contains a contextualized user journey to explore and understand the RMRK Nestable NFT features:

- **[Nestable demo repository](https://github.com/rmrk-team/evm-nestable-demo)**