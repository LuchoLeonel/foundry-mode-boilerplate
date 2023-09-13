# Using Foundry
## How to deploy with Foundry on MODE

This guide will walk you through the deployment of an ERC20 token on MODE using [Foundry](https://book.getfoundry.sh/). Foundry is a smart contract development toolchain written in Rust.
Foundry manages your dependencies, compiles your project, runs tests, deploys, and lets you interact with the chain from the command-line and via Solidity scripts.
Given that MODE is built on the OP Stack and is EVM-compatible, you can easily port any Ethereum-based smart contract without modifying the core code, requiring only minor setting adjustments.

### Prerequisites:
Even if you need to do these 3 steps, it won’t take more than 10 minutes to set up:
- Have some ETH on Mode Testnet Network. You can follow our guide on [how to bridge](https://docs.mode.network/get-started/bridging-to-mode-testnet). Or use [this faucet](https://faucet.modedomains.xyz/) to claim some ETH on Mode.
- Rust must be installed on your computer. If it's not, [follow this guide](https://doc.rust-lang.org/book/ch01-01-installation.html).
- Foundry must be installed on your computer. If it’s not, [follow this guide](https://book.getfoundry.sh/getting-started/installation).

If you just want to skip to the build & deploy section of this tutorial, you can clone this repository that contains a boilerplate Foundry project and skip to step 3.

Let's get started!

## 1. Setting Up The Project
#### 1.1. Initialize a new Foundry project:

Forge is the command-line interface (CLI) tool for Foundry, allowing developers to execute operations directly from the terminal.
Open up a terminal and run this command:

```
forge init my-project
```


#### 1.2. Install the OpenZeppelin contracts library inside your project, which provides a tested and community-audited implementation of ERC20:
```
forge install OpenZeppelin/openzeppelin-contracts
```


## 2. Writing the ERC20 Token Contract
#### 2.1. In /src of your project directory, create a text file named MyERC20.sol and add:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "lib/openzeppelin-contracts/contracts/token/ERC20/ERC20.sol";

contract MyERC20 is ERC20 {
    constructor() ERC20("MyToken", "MTK") {}
}
```
This is a simple ERC20 token named "MyToken" with the symbol "MTK". You can name it any way you want and also write any other contracts. Just remember that if your file name is called differently, the commands to deploy will be slightly different. 
This is what you should have up to now:

![](https://lh6.googleusercontent.com/1jahpfwmFhOAiT_xMNIb0-kux3ARUGOy-3euxZ7_gT6Gg17CnCIGNbaeLIqrisbaNa1ZyJ_npaKhMyHNnyZCZ_fnmWknw3fo4P_1immNKd6fDyWDYc8rU6srodquISlaY64TTpbdar9d57v5RSuWWFk)


## 3. Building The Contract
#### 3.1. Use Foundry to compile your smart contract running the following command:
```
forge build
```

## 4. Deploying the ERC20 Token Contract
#### 4.1. To deploy your contract you will need to run the following command and replace <YOUR_PRIVATE_KEY> with your credentials:
⚠️ Never share or expose your private key publicly. This will grant complete control to whoever has it. Please store it safely.

```
forge create --rpc-url https://sepolia.mode.network --private-key <YOUR_PRIVATE_KEY> src/MyERC20.sol:MyERC20
```

PRO TIP: You can add the --verify flag and use blockscout to verify your contract while deploying. 

The following is the command to deploy and verify the contract. If you already deployed your contract but still want to verify it, don't panic! Next step will walk you through that case.

```
forge create --rpc-url https://sepolia.mode.network --private-key <YOUR_PRIVATE_KEY> src/MyERC20.sol:MyERC20 --verify --verifier blockscout --verifier-url https://sepolia.explorer.mode.network/api\?

```

In the output you should get something like:

```
[⠢] Compiling... No files changed, compilation skipped 
Deployer: 0x3F26b51E23D01b09f4079B2a9e00e6873a8409D8 
Deployed to: 0x628F56856386A4De8414A4D8217D519bF94d03f0 
Transaction hash: 0xbe2d27554f130a720c4dd82dad055c941ca44dee836f6333a8507d76022c158
```

Copy the “Deployed to” value and store it somewhere to use later. This is the address of your contract.


## 5. Verifying the ERC20 Token Contract
#### 5.1. For already deployed contracts you can use the verify-contract command:
```
forge verify-contract <CONTRACT_ADDRESS> src/MyERC20.sol:MyERC20 --verifier blockscout --verifier-url https://sepolia.explorer.mode.network/api\?
```


## 6. Exploring and Interacting with Your Deployed Contract

Use the [blockscout](https://sepolia.explorer.mode.network/) explorer to view your contract's details. Paste your contract's address from the deployment output into blockscout's search bar.

In the "Contract" tab you'll find your verified contract.

![](https://lh5.googleusercontent.com/OJLvZ0NHyU_z-JXDaPXZGfjMhAPsWr0PENTz5FCGAvB89b57lT05jwVJu6CFwW-isDp_5ySWmP55IS4mP7QO9ybM5J0N88dcHLHXSJo_f-IGaXxbb-oEUUGj5mjG6J64Tmb-oxNYKD1A3Xpg6hXZ_gk)

Congratulations, you just deployed and verified a contract on Mode Network using Foundry. 
To learn more about Mode and how to turn your code into a business, join our [Discord](https://discord.gg/ModeNetwork) and say hello 👋

Happy coding! 