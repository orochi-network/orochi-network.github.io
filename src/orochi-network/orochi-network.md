<p align="center">
    <img src="../assets/orochi-network.png" alt="Orochi Network">
</p>

## What is Orochi Network?

Orochi Network is a project that utilizes cryptography, especially Zero-Knowledge Proofs (ZKP) to build up an operating system (UnityOS) for the next generation of the internet.

UnityOS manages a pool of computation power. It allows High Performance Decentralized Applications to be hosted and processed without any centralized server. UnityOS runs in a server-less fashion with the underlying technology that can handle almost everything. Hence, the developers only need to focus on designing the logic of their dApps. This layer is built with distributed computing using [zkWASM](https://github.com/orochi-network/zkWASM-specs).

The dApps can be implemented by popular programming languages, e.g., C++, Rust, Go, etc, and processed with semi-native performance. Computation power will be allocated dynamically and thus, saves operational costs and computational waste.

## Our Vision

At Orochi Network, we believe that Verifiable Computation is a essential cryptographic system to establish Web3 and Decentralized Autonomous Economy. However, in order to reach this stage, there are still a number of major challenges of this industry to overcome.

- **The limits of computation:** _EVM cannot guarantee semi-native performance, in addition to the extremely high cost and latency to perform computations. dApps nowadays are unstable, expensive, slow and unfriendly to the mass. In other words, they are currently unusable and we cannot replace an ordinary application by a dApp yet._

- **Data correctness:** _There is no way to prove the correctness of data since all data pipelines are stored in a blackbox. We have no idea how data are processed._

- **Data availability:** _That smart contract and application executors are isolated from the internet prevents data to be accessible from the run-time environment. One can require a third-party service to feed necessary data. This approach is broken since we cannot verify the data. Moreover, the latency from the third-party services is unacceptable._

## What Are We Building Toward To That vision?

### zkWASM: An Universal Verifiable WebAssembly Run-time

This is considered to be the core component of Orochi Network that provides semi-native executions and proves the computations in ZKPs. We booted this aspect with [zkWASM specifications](https://github.com/orochi-network/zkWASM-specs).

### zkDatabase: A Verifiable Database

[zkDatabase](https://github.com/orochi-network/zkDatabase) is a database that utilizes ZKPs to prove the correctness of the data and data processing. As far as we know, every zkApp needs to manage their own on-chain and off-chain state itself. This process is costly and inefficient depending on the complexity of data's structure. We want to provide other teams the most critical component, namely, the database, to build their zkApps.

### Orand: A Decentralized Random Number Generator

We are conducting various research attempts around [VRF](../ecvrf/verifiable_random_function.md) and [ECVRF](../ecvrf/ecvrf_summary/introduction.md) to develop a solution for mass adoption of verifiable randomness. Verifiable randomness must be considered an essential primitive of Web3 gaming.

Orand is implemented as a core library of [Orochimaru](https://github.com/orochi-network/orochimaru), a full-node client of Orochi Network.

We are also implementing the [on-chain verification](https://github.com/orochi-network/orochimaru/tree/main/contracts) allowing the randomness to be verified without any third-party service. The verifier is implemented in `Solidity` and is EVM-compatible at the moment. In the future, we are planning to support other blockchains that support smart contracts.

### Orosign: A Passport of Web3

A highly secured solution based on Multi-Party Computations (MPC) that helps you secure your digital assets and digital identities. The product is available on Apple Store and Google Play.

## Orochi ❤️ Open Source

All projects are open-sourced and public. We build everything for common good.

<!-- Expect us -->
