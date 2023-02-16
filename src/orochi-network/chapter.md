<p align="center">
    <img src="../assets/orochi-network.png" alt="Orochi Network">
</p>

## What is Orochi Network?

Orochi Network is a project that utilizes cryptography, especially Zero-Knowledge Proofs (ZKP) to build up an operating system (UnityOS) for the next generation of the internet.

UnityOS manages a pool of computation power. It allows High Performance Decentralized Applications to be hosted and processed without any centralized server. UnityOS runs in a server-less fashion with the underlying technology that can handle almost everything. Hence, the developers only need to focus on designing the logic of their dApps. This layer is built with distributed computing using [zkWASM](https://github.com/orochi-network/zkWASM-specs).

The dApp can be implemented by popular programming languages namely C++, Rust, Go, etc, and processed with semi-native performance. Computation power will be allocated dynamically and thus, saves operational costs and computational waste.

## Our Vision

At Orochi Network, we believe that Verifiable Computation is a critical primitive to establish Web3 and Decentralized Autonomous Economy. However, in order to reach this stage, there are still a number of major challenges of this industry to overcome.

- **The limits of computation:** _EVM can not guarantee semi-native performance, in addition to the extremely high cost and latency to perform computation. dApps nowadays are unfriendly to the mass, unstable, expensive and slow. In other word, they are currently unusable and we can not replace an ordinary application by a dApp yet._

- **Data correctness:** _There is no way to prove the correctness of data since all data pipelines are stored in a blackbox. We have no idea how data are processed._

- **Data availability:** _Smart contract executor and application executor are isolated from the internet that prevent data to be accessible from the run-time environment. It alway requires a third party service to feed necessary data. This approach is broken since we can not verify the data. Moreover, the latency from the third parties is unacceptable._

## What Are We Building Toward To That vision?

### zkWASM: An Universal Verifiable WebAssembly Run-time

This consider to be a core component of Orochi Network that provides semi-native execution and proving the computation in ZKP. We booted with [zkWASM specifications](https://github.com/orochi-network/zkWASM-specs).

### zkDatabase: A Verifiable Database

[zkDatabase](https://github.com/orochi-network/zkDatabase) is a database that utilize Zero-Knowledge Proof (ZKP) to prove the correctness of the data and data processing. As far as we know, every zkApp need to manage their own on-chain and off-chain state themselves, this is costly and inefficient depend on the complexity of data's structure. We want to help other team to build their zkApp by provide the most critical component, the database.

### Orand: A Decentralized Random Number Generator

We do various research around [VRF](../ecvrf/verifiable_random_function.md) and [ECVRF](../ecvrf/ecvrf_summary/introduction.md) to develop a solutions for mass adoption of verifiable randomness. Verifiable randomness must be consider as a essential primitive of Web3 gaming.

Orand was implemented as a core library of [Orochimaru](https://github.com/orochi-network/orochimaru) a full-node client of Orochi Network.

We're also implement the [on-chain verification](https://github.com/orochi-network/orochimaru/tree/main/contracts), allow the randomness to be verified without a third party service. The verifier was implemented in `Solidity` and it's EVM compatible at this time. In the future, we're planned to support any blockchains that supported smart contracts.

### Orosign: A Passport of Web3

A high security solution based on Multi-Party Computation that helps you secure your digital assets and your digital identity. The product is available on Apple Store and Google Play.

## Orochi ❤️ Open Source

All projects are open source and public, we built everything for common good.

<!-- Expect us -->
