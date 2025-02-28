---
layout: single
title: Digital Evolutions
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
This is the evolution of digital identity overtime:

![Digital Identity evolution](/assets/2025/02/digital-identity-evolution2.png "Digital Identity evolution")

SSO is a part of integration in an organization system. From Okta, Entra ID, etc., we have SAML2 and OpenID Connect 1.0 providers. However, the world is now moving to Self-Sovereign Identity (SSI). The countries, however, as sovereigns and the protectors of their citizens need to create their own identity.

Currently, Europe and other countries are moving away to SSI with W3C DID (Digital Identifier). They are using Blockchains like Etherium and Sovrin to store the metadata to ensure the integrity of any shared/created digital identifiers. Digital identifiers can be your foundational ID (e-KTP) or your bachelor degree.

The idea of DID using blockchain is to make a certain type of document as a smart contract. Smart contract is actually a little program running inside the blockchain. If I may make analogy, kind of like applet running inside of a smartcard.

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