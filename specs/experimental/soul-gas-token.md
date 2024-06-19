# Soul Gas Token

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Soul Gas Token](#soul-gas-token)
  - [Motivation](#motivation)
  - [How It Works](#how-it-works)
  - [Potential Challenges](#potential-challenges)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## Motivation

To bridge the gap between traditional web users and the growing world of Web3, we propose a **non-transferable, value-less gas token** named SoulGasToken. The concept revolves around facilitating Web2 users' entry into Web3 by airdropping them with SoulGasToken. This token will enable users to pay for transaction gas fees without the immediate need for native gas token or other valuable cryptocurrencies. This initiative is particularly aimed at those new to Web3, providing a seamless transition without the upfront cost of acquiring native gas token.

## How It Works

- **Technical Implementation**: SoulGasToken is a non-transferable ERC20 contract with two mutual *exclusive* modes: it can be either backed by native gas token or only the sequencer can `mint` new tokens. The mode needs to be decided at genesis and can never change after that. To maintain its non-transferable nature, all attempts to use ERC20's transfer, transferFrom, or approve methods will result in failure.
- **Wallet Compatibility**: The transaction format remains consistent with existing Rollup ones. Users can transact using ETH wallets without the need for additional ones (e.g., AA wallets), ensuring a smooth user experience.
- **Gas Fee Process**: For an L2 transaction, the fee will be deducted from the user's SoulGasToken balance if it's sufficient. Otherwise, the system will draw from the user's native gas token balance.

## Potential Challenges

- **Sequencer Incentives**: As the miner of SoulGasToken, there's an inherent expectation for the sequencer to process SoulGasToken transactions, though it may re-order transactions to prioritize transactions paying fees in native gas token over SoulGasToken.
- **Risk of DoS Attacks**: The introduction of a "free" transaction token like SoulGasToken could potentially incur denial-of-service (DoS) type attacks. Mitigation strategies include limiting the initial airdrop quantity to support a finite number of transactions, coupled with a mechanism to replenish SoulGasToken for users with verified on-chain behaviors.