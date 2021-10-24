---
layout: post
title: "Introduction to Block Chain"
date: 2020-03-24
catalog: true
author:  Icy
header-img: img/introductiontoblockchain.jpg
tags: [Block Chain]
---

# Development of protocol and Blockchain[^1]
## Circuit Switching   
Connections between two parties or merchains has to be preestablished and sustained throughout an exchange, which need dedicated lines.  

## TCP/IP(Transmission Control Protocol/Internet Protocol )  
### Introduction  
TCP/IP transmitted information by digitizing it and breaking it up into very small packets with address information. The packets can take any route to the recipient by dissembling and reassembling the packets and interpreting and encoding data.   

### Development of TCP/IP
It develops from single-use case (e-mail) and localization(internal corporate e-mail network) to substitutes of existing businesses (online shop), then to transformative application (eBay).  

## Blockchain  
### Introduction of Blockchain 
Blockchain enables bilateral financial transactions to lower the cost of transactions. In a blockchain system, the ledger is replicated in a large number of identical database, each hosted and maintained by an interested party. When changes are entered in one copy, all the other copies are simultaneously updated. There is no need for third-party intermediaries to verify or transfer ownership.  
### How blockchain works
1. distributed database:   
	- each party on a blockchain has access to the entire database and its complete history. 
	- No single party controls the data or the information.  
	- Every party can verify the records of its transaction partners directly, without an intermediary.  

2. Peer-to-peer transmission  
	- Communication occurs directly between peers without any central node.  
	- Each node stores and forwards information to all other nodes.  

3. Transparency with pseudonymity  
	- Every transaction and its associated value are visible to anyone with access to the system.  
	- Each node on a blockchain has a unique 30-plus-character alphanumeric address that identifies it.  
	- Users can choose to remain anonymous or provide proof of their identity to others.  
	- Transactions occur between blockchain address.  

4. Irreversibility of records  
	- Once a transaction is entered in the database and the accounts are updated, the records cannot be altered.  
	- Various computational algorithms and approaches are deployed to ensure that the recording on the database is permanentm, chronologically ordered, and available to all others on the network.  
5. Computational logic  
	- The digital nature of the ledger means that the blockchain transactions can be tied to computational logic and in essence programmed.  
	- Users can set up algorithms and rules that automatically trigger transactions between nodes.  

### Development of Blockchain (novelty and complexity(coordination) )
- single use: bitcoin payment
- localization: private online ledgers for processing financial transactions 
- substitution: retailer gift cards based on bitcoin
- transformation: self-execting smart contracts  

### How to create [Smart Contracts](https://kauri.io/understanding-smart-contract-compilation-and-deplo/973c5f54c4434bb1b0160cff8c695369/a)
1. Smart contract is written in a human friendly language (e.g Solidity)  
2. The code is compiled into bytecode and a set of function descriptors (Application Binary Interface, known as ABI) by a compiler (e.g Solc)  
3. Development Blockchain  
	- [Geth](https://github.com/ethereum/go-ethereum/wiki/geth) & [Ganache-CLI](https://github.com/trufflesuite/ganache-cli): Ethereum environments to deploy our smart contract(Geth: compile json file which is the output of Solc; Ganache-CLI: listen)  
	- Starting Ganache-CLI & attaching Geth Console
3. The bytecode is packed with other parameters into a transaction  
4. The transaction is signed by the account deploying the contract
5. The signed transaction is sent to the blockchain and mined

### Books for Blockchain   
- Mastering Bitcoin: Unlocking Digital Cryptocurrencies by Andreas M. Antonopoulos  

### Platforms of Blockchain   
- [Ethereum](https://ethereum.org/): a global, open-source platform for decentralized applications, based on GO-language 
- [Qtum](https://qtum.org/zh)
- [Hyperledger](https://www.hyperledger.org/), based on GO-language 
- [Multichain](https://www.multichain.com/)
- [Loom](https://cryptozombies.io/)  
- [Bitcoin](https://github.com/bitcoin/bitcoin)

### Coding language of blockchain  
- [Solidity](https://solidity.readthedocs.io/en/latest/installing-solidity.html)  

### Developer Tools of blockchain[^2]  
1. Fore-end:  
	- Java  
	- HTML  
2. BackendFramework:  
	- [Truffle](https://www.trufflesuite.com/) - A development environment, testing framework, build pipeline, and other tools.  
	- [Embark](https://embark.status.im/docs/) - A development environment, testing framework, and other tools integrated with Ethereum, IPFS, and Whisper.
	- [Waffle](https://getwaffle.io/) - A framework for advanced smart contract development and testing (based on ethers.js).
	- [Python Tooling](http://python.ethereum.org/) - Variety of libraries for Ethereum interaction via Python.  
	- [Brownie](https://eth-brownie.readthedocs.io/en/latest/) - Python-based development environment and testing framework.  
3. Data Structure
	- Stack  
	- Queue  
	- Linked List  
	- Tree  
	- HashMaps  

### Learning Resources of blockchain[^2]:
1. Lynda: 
	- [Foundation](https://www.lynda.com/JavaScript-tutorials/Foundations-of-Programming-Fundamentals/83603-2.html)
	- [JavaScript](https://www.lynda.com/JavaScript-tutorials/JavaScript-Essential-Training/574716-2.html)
	- [Data structure](https://www.lynda.com/Developer-Programming-Foundations-tutorials/Foundations-Programming-Data-Structures/149042-2.html)  
	- [Foundations Programming of Discrete Mathematics](https://www.lynda.com/Programming-Foundations-tutorials/Foundations-Programming-Discrete-Mathematics/411376-2.html)  
	- [Git](https://www.lynda.com/Git-tutorials/Git-Essential-Training/100222-2.html)  
	- [Foundations Programming of Refactoring Code](https://www.lynda.com/Developer-Programming-Foundations-tutorials/Foundations-Programming-Refactoring-Code/122457-2.html)  
	- [Introduction to Cryptography](https://www.youtube.com/channel/UC1usFRN4LCMcfIV7UjHNuQg/videos)  
	- [Game theory](https://www.youtube.com/playlist?list=PL6EF60E1027E1A10B)  
	- [Bitcoin and Cryptocurrency Technologies Online Course](https://www.youtube.com/channel/UCNcSSleedtfyDuhBvOQzFzQ/videos)
	- [Solidity-1](https://learnxinyminutes.com/docs/solidity/)  
	- [Solidity-2](https://docs.erisindustries.com/tutorials/solidity/) 
	- [Ethereum](https://www.youtube.com/channel/UC6rYoXJ_3BbPyWx_GQDDRRQ) 
2. Coursera  
	- [Crypto and Cryptocurrencies](https://www.coursera.org/learn/cryptocurrency)  
3. Stanford 
	- [CS 251: Cryptocurrencies and Blockchain Technologies](https://cs251crypto.stanford.edu/18au-cs251/) 

# Reference
[^1]: MARCO IANSITI, KARIM R.LAKHANI. The truth about blockchain, JANUARY–FEBRUARY 2017 HARVARD BUSINESS REVIEW  
[^2]: https://mp.weixin.qq.com/s/HG3xnU-m88CpmIgvd6YhPw

