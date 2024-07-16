# Extend the Availability of EIP-4844 Blobs by Integrating OP Stack with EthStorage

## Motivation

This spec aims to offer a complete long-term data availability (DA) solution for OP Stack EIP-4844 (Ecotone) upgrade by integrating with EthStorage. EthStorage is a permanent long-term DA layer for EIP-4844 binary large objects (BLOBs) powered by its invented proof of data availability sampling over time algorithm. EthStorage has been deployed on Ethereum DenCun testnets and integrated EIP-4844.  This helps OP Stack Rollups better derive Ethereum security even after the BLOBs are pruned after 18 days as specified by EIP-4844.

## How It Works

The integration mainly includes two parts:
1. The [`BatchInboxAddress`](https://github.com/ethereum-optimism/optimism/blob/db107794c0b755bc38a8c62f11c49320c95c73db/op-chain-ops/genesis/config.go#L77) is replaced by an inbox contract to call EthStorage storage contract’s [`putBlob`](https://github.com/ethstorage/storage-contracts-v1/blob/40e0fdb4be14ac0e340a53c854d9a343aa272ac4/contracts/EthStorageContract.sol#L254) method to store a BLOB permanently.
2. When deriving:
   1. The [`--l1.beacon-fallbacks`](https://github.com/ethereum-optimism/optimism/blob/db107794c0b755bc38a8c62f11c49320c95c73db/op-node/flags/flags.go#L85) cli parameter is specified for op-node in addition to the [`--l1.beacon`](https://github.com/ethereum-optimism/optimism/blob/db107794c0b755bc38a8c62f11c49320c95c73db/op-node/flags/flags.go#L70) cli parameter to fetch expired BLOBs from EthStorage's BLOB archiver node.
   2. As EthStorage's BLOB archiver node only stores BLOBs that's successfully paid via EthStorage storage contract’s [`putBlob`](https://github.com/ethstorage/storage-contracts-v1/blob/40e0fdb4be14ac0e340a53c854d9a343aa272ac4/contracts/EthStorageContract.sol#L254) method, we also need to exclude failed batches.



## Reference Implementation

1. [inbox contract](https://github.com/blockchaindevsh/es-op-batchinbox/blob/main/src/BatchInbox.sol)
2. [op-node derive changes](https://github.com/ethstorage/optimism/pull/22)