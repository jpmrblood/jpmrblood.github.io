---
layout: single
title: Digital Identity Evolutions
tags:
  - did
  - identity
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/02/digital-identity-evolution.png
  overlay_image: /assets/2025/02/digital-identity-evolution.png
excerpt: A road for SSI Digital ID
---

# Evolution 

This is the evolution of digital identity overtime:

![Digital Identity evolution](/assets/2025/02/digital-identity-evolution2.png "Digital Identity evolution")

SSO is a part of integration in an organization system. From Okta, Entra ID, etc., we have SAML2 and OpenID Connect 1.0 providers. However, the world is now moving to Self-Sovereign Identity (SSI). The countries, however, as sovereigns and the protectors of their citizens need to create their own identity.

Currently, Europe and other countries are moving away to SSI with W3C DID (Digital Identifier). They are using Blockchains like Etherium and Sovrin to store the metadata to ensure the integrity of any shared/created digital identifiers. Digital identifiers can be your foundational ID (e-KTP) or your bachelor degree.

The idea of DID using blockchain is to make a certain type of document as a smart contract. Smart contract is actually a little program running inside the blockchain. If I may make analogy, kind of like applet running inside of a smartcard.

# Terminology

| Terminology| Description|
|---------|------------|
| Digital Identifier (DID) | A unique piece of data or code used to represent an individual, organization, object, or entity in a digital or online environment. Digital identifiers are essential for authentication, authorization, and tracking in various systems and platforms, helping to ensure that entities can be accurately recognized and differentiated in the digital space.|
| Foundational ID | A legal and authoritative identification system that provides a universal, official, and verifiable identity to individuals, typically issued by a government or a central authority. Foundational IDs serve as the primary identification method that can be used across various sectors, services, and systems, such as healthcare, education, banking, and social services. In Indonesia this is NIK/KIA/KITAS |
| Blockchain | A decentralized and distributed digital ledger that records transactions across multiple computers in a way that ensures the data is secure, transparent, and immutable. Example of blockchain: Bitcoin, Etherium, Dogecoin, Sovrin, Cheqd |
| Blockchain network | A blockchain network is a decentralized system consisting of multiple nodes (computers) that collaborate to maintain a distributed digital ledger, which records transactions and data across the network. The main characteristic of a blockchain network is that it operates without a central authority, relying on peer-to-peer communication and consensus mechanisms to validate and secure the information on the ledger. |
| Immutable | Cannot be modified |
| Transaction | An operation recorded on the blockchain, such as transfers of assets, data entries, or execution of smart contracts. Transactions are grouped into blocks, which are then added to the blockchain. |
| Block | Each block in the blockchain contains a list of transactions, a timestamp, and a cryptographic hash linking it to the previous block. This structure ensures data integrity and immutability. |
| Smart Contract | A self-executing contract with the terms of the agreement between buyer and seller directly written into lines of code. These contracts automatically enforce and execute the terms when predetermined conditions are met, without the need for intermediaries (such as banks, lawyers, or notaries).|
| Cryptographic Security| Blockchain networks use cryptographic techniques (such as public-key cryptography and hashing) to secure transactions, verify data authenticity, and ensure the integrity of the ledger. |
| Node | Independent participants in the network (computers, servers, or devices) that maintain and share the blockchain’s data. Nodes validate transactions, store the blockchain’s data, and may participate in consensus mechanisms. |
| Blockchain Ledger | A distributed, immutable ledger that stores all transactions or data entries. Each block in the blockchain contains a set of transactions, a timestamp, and a reference (hash) to the previous block, forming a chain of data blocks. |
| Consensus Mechanism | A protocol for nodes to agree on the blockchain state. |

## Type of Network 

| **Type of Blockchain**  | **Description**                                                                                     | **Examples**                       | **Use Case**                                       | **Consensus Mechanism**                          |
|-------------------------|-----------------------------------------------------------------------------------------------------|------------------------------------|---------------------------------------------------|--------------------------------------------------|
| **Public Blockchain**    | Open and Permissionless: Anyone can join and participate without needing permission.                 | Bitcoin, Ethereum, Solana          | Cryptocurrencies, DeFi, open platforms             | Proof of Work (PoW), Proof of Stake (PoS)        |
| **Private Blockchain**   | Closed and Permissioned: Only authorized entities can read and write to the ledger.                 | Hyperledger Fabric, Corda          | Enterprise applications, supply chain, private transactions | Practical Byzantine Fault Tolerance (PBFT)       |
| **Consortium Blockchain**| Partially Decentralized: Managed by a group of pre-selected organizations, sharing control.          | Quorum, R3 Corda                   | Cross-industry collaboration, banking, trade finance | Raft, Practical Byzantine Fault Tolerance (PBFT) |
| **Hybrid Blockchain** | Combination of public and private blockchain to allow for both permissioned and permissionless access, enabling flexibility in sharing data and controlling access. | Dragonchain, XinFin | Organizations that want to keep certain data private but allow access to public information. | depends. |

## Consensus Mechnism

| Terminology| Description|
|---------|------------|
| Proof of Work (PoW) | Requires participants (miners) to solve complex mathematical puzzles to validate transactions and create new blocks (used in Bitcoin). |
| Proof of Stake (PoS) | Validators are chosen to create new blocks based on the number of coins they hold and are willing to "stake" as collateral (used in Ethereum 2.0). |
| Delegated Proof of Stake (DPoS) | Stakeholders vote for delegates who are responsible for validating blocks on behalf of the network. | 


# Language for Smart Contract

The smart contract has many programming language. The most known is `Solidity`. Based on [Top 6 Smart Contract Languages in 2024](https://chain.link/education-hub/smart-contract-programming-languages) here's the list of languages:

| Language        | Description                                                                 | Platforms Supported                                                                 |
|-----------------|-----------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| Solidity        | A high-level language for Ethereum smart contracts.                         | Ethereum, Binance Smart Chain, Avalanche C-Chain, Fantom, and more.               |
| Vyper           | A Pythonic language for Ethereum smart contracts.                           | Ethereum.                                                                         |
| Rust            | A systems programming language with smart contract frameworks.              | Solana, NEAR Protocol, Polkadot (Substrate), and more.                           |
| Go              | A statically typed language used for smart contracts.                       | Ethereum (via Geth), Hyperledger Fabric, and more.                                |
| JavaScript      | A versatile language used for smart contracts.                             | Ethereum (via Node.js), Hyperledger Fabric, and more.                             |
| Michelson       | A low-level stack-based language for Tezos smart contracts.                 | Tezos.                                                                            |
| Clarity         | A decidable language for smart contracts.                                  | Stacks.                                                                          |
| Move            | A language for smart contracts with a focus on safety and flexibility.      | Diem (formerly Libra).                                                            |
| Bamboo          | A language for smart contracts with a focus on simplicity and security.     | Ethereum.                                                                         |
| Ligo            | A high-level language for Tezos smart contracts.                           | Tezos.                                                                            |
| Simplicity      | A low-level language for Bitcoin smart contracts.                          | Bitcoin.                                                                         |
| Solidity++      | An extension of Solidity with additional features.                         | Ethereum.                                                                         |

We need to create a definition of smart contract. The problem in Indonesia is besides the knowledge gap is also about the gas cost of using blockchain. No public blockchain running freely. We need to pay for each identifier published. Who's gonna pay this?

# Regulation Needed

We need to create a business process for this.

Also, because we are going to build a Digital Identity infrastructure, we need to make sure that we have the regulation in place. Some of the questions that I managed to capture:
- What standard we are going to use? (W3C DID, OpenID SIOPP, AnonCreds, SD-JWT, MDL 2.0, etc.)
- Who's going to run the blockchain?
  - If government to participate, under which government organization/body? (I think it's best to have multiple government body runs the node)
  - If we open to commercial/for-profit organization, how are we going to manage the business?
  - What is the security threat model?
- Who's going to be the wallet?
  - We have UU PDP chapter 13 for enabling user wallet.
  - Can we let the non-government ID wallet runs in Indonesia?
- And so on.

Oh, boy, such a long road ahead.