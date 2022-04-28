# Base blockchain ethereum concept
---
## Glossary

**Account** - an object containing an address, balance, nonce, and optional storage and code. An account can be a contract account or an externally owned account (EOA).  

**Address** - most generally, this represents an EOA or contract that can receive (destination address) or send (source address) transactions on the blockchain. More specifically, it is the rightmost 160 bits of a Keccak hash of an ECDSA public key.  

**Nonce** - in cryptography, a value that can only be used once. There are two types of nonce used in Ethereum - an account nonce is a transaction counter in each account, which is used to prevent replay attacks; a proof-of-work nonce is the random value in a block that was used to satisfy the proof-of-work.  

**Block** - a collection of required information (a block header) about the comprised transactions, and a set of other block headers known as ommers. Blocks are added to the Ethereum network by miners.

**Blockchain** - in Ethereum, a sequence of blocks validated by the proof-of-work system, each linking to its predecessor all the way to the genesis block. There is no block size limit; it instead uses varying gas limits.

**Gas** - a virtual fuel used in Ethereum to execute smart contracts. The EVM uses an accounting mechanism to measure the consumption of gas and limit the consumption of computing resources (see Turing complete).

**Gas limit** - the maximum amount of gas a transaction or block may consume.

**Network** - referring to the Ethereum network, a peer-to-peer network that propagates transactions and blocks to every Ethereum node (network participant).

**Node** - a software client that participates in the network.

## Accounts 
[Metamask valet]([https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn/related) - metamask valet uses like an tool for authorization vs authentication.
Creates metamask account by using 12 words (mnemonic) - it creates big counts of accounts. It's usable for switching between dev/prod networks and accounts.
[https://iancoleman.io/bip39/](https://iancoleman.io/bip39/) here is information of accounts created with mnemonics. 

### Adding a test money
[rinkeby-faucet](https://rinkeby-faucet.com/)
[https://faucet.rinkeby.io/](https://faucet.rinkeby.io/)

## Blockchain
[core concept live demo](https://andersbrownworth.com/blockchain/)

## Solidity Online IDE
[Remix](https://remix.ethereum.org/)
Uses Javascript VM for emulated and fast execution of contracts.

## Links
[Official glossary](https://ethereum.org/en/glossary/)  
[Metamask](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn/related)  
[Add demo money: Rinkeby-faucet](https://rinkeby-faucet.com/)  
[One more service for adding demo moneq to Rinkeby network](https://faucet.rinkeby.io/)  
[core concept live demo](https://andersbrownworth.com/blockchain/)  
[Mnemonic-to-accounts information](https://iancoleman.io/bip39/)  
