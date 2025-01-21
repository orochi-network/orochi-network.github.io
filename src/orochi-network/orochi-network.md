<p align="center">
    <img src="../assets/orochi-network.png" alt="Orochi Network">
</p>

# Orochi Network: Verifiable Data Infrastructure

## Challenges

### Data Integrity

In a truly decentralized Web3 ecosystem, data integrity would be ensured through a distributed network of aggregators. Each node could independently prove the correctness of data using cryptographic protocols.

However, many Web3 solutions today still rely on oracles, which are fundamentally flawed and unable to guarantee data authenticity. Consequently, smart contracts are often unable to confirm the legitimacy of third-party provided data. This vulnerability leads to potential losses and fraudulent activities.

### Data Availability

Smart contracts run in an isolated environment, such as the EVM or WASM runtime on the blockchain. While this isolation allow smart contract to be executed seamlessly regardless the differences of the architecture, it also limits their ability to directly interact with external data sources, like those in the real world.

Additionally, as the number of users and transactions on a blockchain network grows, storing and accessing all data on-chain becomes increasingly impractical and costly.

### Interoperability

Interoperability is a critical aspect of any decentralized Web3 ecosystem. However, many existing solutions struggle with interoperability due to the lack of standardization and compatibility between different architectures.

Existing DA Layer solution is just a combination of blockchain and commitment schemes and it is failed to prove the DA state to on-chain contracts in a single succinct proof.

### Scalability

Nowadays DA Layers are leveraging existing technical stack that mean they are also inherits issues of existing blockchains, namely finality and scalability. They can not deallocate resource that store on their system and unable to reach instant finality with BFT consensus.

## Our Solutions

### Orochi Network: Verifiable Data Infrastructure

Orochi Network positions as the first Verifiable Data Infrastructure, emphasizing the use of ZKPs for secure and verifiable data processing. This focus on ZKPs caters to applications, platforms requiring high levels of privacy, security and decentralized. Here's a breakdown of our key features and potential of our ZKP centric approach:

- **Native ZK-data-rollups:** Unlike other DA Layers, Orochi Network natively supports ZKPs and perform the rollups on the data. This allows for efficient on-chain verification of data with one single succinct proof, this approach potentially improving scalability and privacy for decentralized applications.
- **Verifiable Data Pipeline:** Orochi Network goes beyond just data availability. We offers cryptographic proofs at every step of data processing – from sampling to storage and retrieval. Our solution is only reply on cryptography protocols that helps to take down third party trust and helping to transform **Real World Data** to **Provable Data** which can be read and verified by smart contracts.
- **Utilizes Merkle Directed Acyclic Graph (Merkle DAG):** Orochi Network leverages Merkle DAG technology, potentially offering advantages over traditional blockchain structures in terms of scalability and performance.
- **Succinct Hybrid aBFT Consensus:** This consensus mechanism allows for asynchronous finalization of states, potentially improving efficiency compared to synchronous approaches used by some competitors.
- **Proof-System Agnostic:** Orochi Network can work with various ZKP systems like Plonky3, Halo2, Nova, and Pickles, offering developers flexibility in choosing the most suitable proof system for their needs.
- **Blockchain Agnostic:** Orochi Network is designed to be blockchain-agnostic by leveraging ZKPs to improve interoperability between different blockchains, potentially enabling integration with diverse blockchain platforms.

#### Overview

zk-SNARK is used as a succinct proof for on-chain and off-chain interoperability; it establishes cryptographic verification. We also use this advantage to perform layer-to-layer interactions. Our Verifiable Data Infrastructure supports ZKPs natively, for each blockchain we will use commitment schemes and ZKPs that are most compatible with a given platform. This approach allows a huge leap in performance adn compatibility.

```
                                ┌──────────────────────┐
                                │    Execution Layer   │
                                └──────────▲───────────┘
                                           │
                                           │Data & ZKP
                      Verifiable           │
 ┌───────────────────┐ Sampling ┌──────────┴───────────┐
 │  Real World Data  ┼──────────►          VDI*        │
 └───────────────────┘          └──────────┬───────────┘
                                           │
                                           │Commit
                                           │
                                ┌──────────▼───────────┐
                                │   Settlement Layer   │
                                └──────────────────────┘

(*): Verifiable Data Infrastructure
```

#### Verifiable Data Pipeline

We not only support ZKPs natively, but also generate cryptographic proof of data integrity. We solve the most challenging problem where **Real World Data** isn’t provable to smart contracts and dApps. **Verifiable Data Pipeline** opens the door to the future of **Provable Data**.

```

  ┌────────────┐                    ┌────────────┐
  │            │                    │            │
  │ Verifiable │      Sampling      │ Verifiable │
  │  Sampling  ├───────────────────►│ Processing │
  │            │                    │            │
  └────────────┘                    └──────┬─────┘
                                           │
                                           │
                                           │ Store
                                           │
                                           │
                                    ┌──────▼─────┐
                                    │            │
                        ┌───────────┤ Immutable  ◄────────────┐
                        │           │  Storage   │            │
                        │           │            │            │
                        │           └────────────┘            │
                        │ Lookup                              │ Update data with ZKP
                        │                                     │
                        │                                     │
                        │                                     │
                 ┌──────▼─────┐                        ┌──────┴─────┐
                 │            │                        │            │
                 │   Lookup   │                        │ Transform  │
                 │   Prover   │                        │   Prover   │
                 │            │                        │            │
                 └──────┬─────┘                        └──────▲─────┘
                        │                                     │
                        │                                     │
      Read data and ZKP │                                     │ Update data & schema
                        │  ┌──────────────────────────────┐   │
                        │  │                              │   │
                        └──►        Execution Layer       ├───┘
                           │                              │
                           └──────────────────────────────┘
```

#### ZK-data-rollups

We all have witnessed ZK-rollups in action. Many projects are utilizing ZKPs to build ZK-rollups and succeed bundle thousands of transactions/executions in one single succinct proof that can be efficiently verified on-chain, we utilized the same property of ZK-rollups to apply on data.

```
 ┌───┬───────────────────┐ x
 │ W │ Data transaction  │ xxx
 └───┴───────────────────┘    xxxxx
 ┌───┬───────────────────┐        xxxxx   ┌───┬───┐
 │ U │ Data transaction  │            xxx │ W │ U │
 └───┴───────────────────┘                ├───┼───┤
 ┌───┬───────────────────┐            xxx │ R │ D │
 │ R │ Data transaction  │        xxxxx   └───┴───┘
 └───┴───────────────────┘    xxxxx        zk-SNARK
 ┌───┬───────────────────┐ xxx              Proof
 │ D │ Data transaction  │ x
 └───┴───────────────────┘

```

## The Future of Web3

Orochi Network's Verifiable Data Infrastructure is a promising step towards a more secure, scalable, and user-friendly Web3. By leveraging the power of Zero-Knowledge Proofs, Verifiable Data Infrastructure offers solutions to some of the most pressing challenges facing the decentralized future of the internet. As Verifiable Data Infrastructure continues to evolve, it has the potential to be a game-changer for Web3, ushering in a new era of innovation and user adoption.

In essence, our suite of products, anchored by the innovative Verifiable Data Infrastructure, lays the groundwork for a future web built on secure, scalable, and user-friendly decentralized applications. By addressing the limitations of current dApps, Orochi Network has the potential to unlock the true potential of Web3, paving the way for a more decentralized and empowering online experience for everyone. The promise of Orochi Network has been recognized by leading organizations within the blockchain space. Orochi Network is a grantee of the **Ethereum Foundation**, **Web3 Foundation**, **Mina Protocol**, and **Aleo**. This recognition underscores the potential of our technology to shape the future of Web3.

## Orochi ❤️ Open Source

All projects are open-sourced and public. We build everything for the common good.

- Our scientific paper that proposes Conditional Folding Scheme: [RAMenPaSTA: Parallelizable Scalable Transparent Arguments of Knowledge for RAM Programs](https://eprint.iacr.org/2024/336)
- Our construction in Distributed ECVRF - [Orand - A fast, publicly verifiable, scalable decentralized random number generator for blockchain-based applications](https://docsend.com/view/5y7rc5cww2juudzn)
- Our zkDatabase - [zkDatabase - Self-proving Database](https://github.com/orochi-network/zkDatabase)
- Our zkVM framework PoC - [zkMemory - An universal memory prover in Zero-Knowledge Proof](https://github.com/orochi-network/orochimaru/tree/main/zkmemory)
- Our proposal to improve security of Smart Contracts - [ERC-6366: Permission Token](https://eips.ethereum.org/EIPS/eip-6366)
- Our proposal to improve permission and role handling - [ERC-6617: Bit Based Permission](https://eips.ethereum.org/EIPS/eip-6617)

#### Other Products of Orochi Network

- [zkDatabase - Self-proving Database](../zkdatabase/chapter.md)
- [zkMemory - An Universal Memory Prover Module](../zk-memory/chapter.md)
- [Orocle and Orand](./orand-orocle.md)

_built with ❤️ and 🦀_
