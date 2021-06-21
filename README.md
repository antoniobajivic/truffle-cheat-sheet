# Truffle configuration cheat sheet

## What is Truffle
As their official site says, truffle is the most popular development framework for Ethereum 
with a mission to make your life a whole lot easier. 

Some of it's features are:
  * Smart Lifecycle Management
  * Automated Contract Testing
  * Scriptable Deployment & Migrations
  * Simple Network Management
  * Powerful Interactive Console
  * External Script Runner

Official Site: <https://www.trufflesuite.com/truffle>

## Prerequisites
Some things are needed before starting to use Truffle:
  * Node version: v12.22.1
  * NPM version: v7.9.0
  * Packages for project:
    * web3: v1.3.5
    * @truffle/hdwallet-provider v1.2.6
  * Account on Infura: <https://infura.io/>
    * Make project on Ethereum network, set Endpoints for your development network

## Get Truffle
Run this command in terminal to get truffle
```bash
npm install truffle -g
```

## Setup Configuration
Inside your root project file there should be a file __truffle.js__ or (for Windows compatibility) __truffle-config.js__  and
it should something like this:
```javascript
const HDWalletProvider = require("@truffle/hdwallet-provider");
const infuraKey = "INFURA_KEY_FROM_SETTINGS"
const mnemonic = "MNEMONIC_FROM_YOUR_ETHEREUM_ACCOUNT" // Mnemonic for your wallet (e.g. Metamask)
module.exports = {
	networks: {
		development: {
			host: "127.0.0.1",
			port: 8545,
			network_id: "*"
			// websockets: true -> This depends on your project needs
		},
		// Ethereum networks configuration (e.g. Rinkeby)
		rinkeby: {
			provider: () => new HDWalletProvider(mnemonic, `https://rinkeby.infura.io/v3/${infuraKey}`),
			      	// gas: 5500000,
      				// gasPrice: 200000000000,
      				// timeoutBlocks: 200,  // # of blocks before a deployment times out  (minimum/default: 50)
      				// skipDryRun: true     // Skip dry run before migrations? (default: false for public nets )
      				network_id: 4,       // rinkeby's id
      				gasPrice: 21000000000,
      				skipDryRun: true,
		},
		mocha: {
    			// timeout: 100000
  		},
		compilers: {
    			solc: {
      				version: "0.8.0",    // Fetch exact version from solc-bin (default: truffle's version)
      				// docker: true,        // Use "0.5.1" you've installed locally with docker (default: false)
      				// settings: {          // See the solidity docs for advice about optimization and evmVersion
      				//  optimizer: {
      				//    enabled: false,
      				//    runs: 200
     				//  },
      				//  evmVersion: "byzantium"
      				// }
    			}
  		}
	}
}

```

## Smart Contracts Deployment

Every projects needs to have migrations folder and __js__ files for instructions to deploy your contract.
Names for each file have few rules, it needs to have number to know order of migrations instructions. 

Example: `__1_initial_migration.js__`

Instructions for deploying __Migrations.sol__ contract:
```javascript
var Migrations = artifacts.require("./Migrations.sol");

module.exports = function(deployer) {
  deployer.deploy(Migrations);
};
```

## Smart Contract File
Every smart contract must have this on the beginning of the file
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
```

## Start using Truffle
Go inside your project path and run this in your terminal
```bash
truffle develop
```

As a result you'll get 10 different accounts with 100 ETH on each account ready for testing purposes, 
their private keys and mnemonic phrase. 
Output should look something like this (yours will differ):
```bash
Truffle Develop started at http://127.0.0.1:9545/

Accounts:
(0) 0x1d7936d8f386a366e954a7d494a0a78423a47b5a
(1) 0x6e4997ff2b0aa953119dcffd3f875146dc0ca79a
(2) 0x92cd10b65115e089d25e8eedfd9111a0e05f2db9
(3) 0x8d964a5666db47a7ba445edb6e205626330ac579
(4) 0x0019777fd45cf46cdf9168477472d67efc47d0db
(5) 0x9481b0f3198944a8da71a56cb79036f6f8b9d3a8
(6) 0xbdfcde440dee9700785757176fd5b1aa7e98470e
(7) 0xf3e8c28127ce30afd23e6de4ee97345bb0673704
(8) 0xa78439af57a458d0519c98f4d86e8a86db1f4806
(9) 0x20e9e6c25d67ce7290bf80ffd46222aeaa4617da

Private Keys:
(0) ebe8b6a9f3f98058d77a16ea225ad16cec426839d6b2ab2cfc633789585f1c5d
(1) 3204cde7e11e7d10a39976a2641868f9fe20be3916b8a8e1d184e78de3f7e285
(2) 3e923a90f6fce0347c2af8ef17ab300e252ec58b75e5a2dba666552fdf7c949e
(3) 5b668dff0f9a0735dc1020b150ba3bf7df6bb3e6b2c1f9a87273fea3e60fa530
(4) 75b9591eface6773cbbb0eae5e14b0420a11947f110eb6d461f0d72290339281
(5) 16ff4ab9dfd6a7a0680e157dbd65daff26d4623ea8346da559afc47f0cb97059
(6) 77621a889e95fda90ed86a51d61cbe06e2ebf0e38fd6037ac904ae559a9c029f
(7) 2ac27b995274362253cb62bd27fd7198d9444d88d681e9bd63e4b60e26d6ce1c
(8) bf345282686047e358add4062c4cfd54819b898f31bffff7e80b7659789be76f
(9) d0b0e719465969978abcf8d79f2636301a426ad905870c28a08109524f822d0c

Mnemonic: best defy weather hobby depart wreck other stomach dragon people notice flame

⚠️  Important ⚠️  : This mnemonic was created for you by Truffle. It is not secure.
Ensure you do not use it on production blockchains, or else you risk losing funds.
```

## Testing
Every app should be tested before deployment and using so create a folder inside your project named __test__ and inside that folder
create a JavaScript file and write your tests. After you write your tests run this command in your terminal, truffle will automatically
compile your contracts:
```bash
test
```

Successful output:
```bash
truffle(develop)> test
Using network 'develop'.


Compiling your contracts...
===========================
> Compiling .\contracts\MyContract.sol
> Artifacts written to C:\Users\YOUR_USERNAME\AppData\Local\Temp\test--18560-fjAi1PfOx90G
> Compiled successfully using:
   - solc: 0.8.0+commit.c7dfd78e.Emscripten.clang

  Contract: MyContract
    √ Success result of your test (2076ms)


  1 passing (2s)
```


## Deployment

For development you should run this command to check if anything is wrong with your smart contracts and for compiling
contracts so you could use your contracts on some of Ethereum networks (e.g. Rinkeby test net)
```bash
compile
```

If everything went okay output should look something like this:
```bash
truffle(develop)> compile

Compiling your contracts...
===========================
> Compiling .\contracts\Migrations.sol
> Artifacts written to C:\Users\YOUR_USERNAME
> Compiled successfully using:
   - solc: 0.8.0+commit.c7dfd78e.Emscripten.clang
```

Deploy to the Rinkeby network by running this command:
```bash
truffle migrate --network rinkeby
```

## Useful Links
* ["How to deploy a smart contract on a public test network (Rinkeby) using INFURA + TRUFFLE + truffle-hdwallet-provider"](https://andresaaap.medium.com/how-to-deploy-a-smart-contract-on-a-public-test-network-rinkeby-using-infura-truffle-8e19253870c4)
* Truffle Pet Shop [Tutorial](https://www.trufflesuite.com/tutorial)
* [Web3](https://web3js.readthedocs.io/en/v1.3.4/) Documentation
* Package Documentation [@truffle/hdwallet-provider](https://www.npmjs.com/package/@truffle/hdwallet-provider)
* [Solidity Cheat Sheet](https://github.com/dekadentno/solidity-cheatsheet)





