# How to deploy with Foundry on MODE

### MODE is a Layer 2 solution on the OP Stack designed to foster growth by incentivizing and rewarding developers, users, and protocols. Given that MODE is built on the OP Stack and is EVM-compatible, developers can easily port Ethereum-based smart contracts without modifying their core code, requiring only minor setting adjustments.

This guide will walk you through the deployment of an ERC20 token on MODE using [Foundry](https://book.getfoundry.sh/).

Prerequisites:
- You should have bridged Sepolia ETH to the Mode Testnet Network.
- Rust must be installed on your computer. If it's not, [follow this guide](https://doc.rust-lang.org/book/ch01-01-installation.html).

## 1. Setting Up The Project
#### 1.1. Initialize a new Foundry project:
```
forge init my-project
```

Forge is the command-line interface (CLI) tool for Foundry, allowing developers to execute operations directly from the terminal.


#### 1.2. Install the OpenZeppelin contracts library inside your project, which provides a tested and community-audited implementation of ERC20:
```
forge install OpenZeppelin/openzeppelin-contracts
```


## 2. Writing the ERC20 Token Contract
#### 2.1. In /src of your project directory, create a file named MyERC20.sol and add::
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "lib/openzeppelin-contracts/contracts/token/ERC20/ERC20.sol";

contract MyERC20 is ERC20 {
    constructor() ERC20("MyToken", "MTK") {}
}
```
This is a simple ERC20 token named "MyToken" with the symbol "MTK".



## 3. Building The Contract
#### 3.1. Use Foundry to compile your smart contract:
```
forge build
```

## 4. Deploying the ERC20 Token Contract
#### 4.1. Deploy with the following command. Replace <YOUR_PRIVATE_KEY> and <ETHERSCAN_API_KEY> with your credentials:
```
forge create --rpc-url https://sepolia.mode.network --private-key <YOUR_PRIVATE_KEY> --etherscan-api-key <ETHERSCAN_API_KEY> src/MyERC20.sol:MyERC20
```
⚠️ Caution: Never share or expose your private key publicly.

If you don’t know how to create an Etherscan api key, you can [follow this guide](https://docs.etherscan.io/getting-started/viewing-api-usage-statistics).


## 5. Verifying the ERC20 Token Contract
#### 5.1. If not yet deployed, add --verify to the previous command:
```
forge create --rpc-url https://sepolia.mode.network --private-key <YOUR_PRIVATE_KEY> --etherscan-api-key <ETHERSCAN_API_KEY> --verify src/MyERC20.sol:MyERC20
```

#### 5.2. For already deployed contracts you can use the verify-contract command:
```
forge verify-contract --compiler-version v0.8.00+commit.c7dfd78e --chain-id 919 --verifier <ETHERSCAN_API_KEY> <CONTRACT_ADDRESS> src/MyERC20.sol:MyERC20
```

## 6. Exploring and Interacting with Your Deployed Contract

Use the [blockscout](https://sepolia.explorer.mode.network/) explorer to view your contract's details. Paste your contract's address from the deployment output into blockscout's search.


With these steps, you should have successfully deployed your ERC20 token on the Mode network using Foundry. Happy coding!