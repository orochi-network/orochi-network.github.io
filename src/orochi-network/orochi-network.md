<p align="center">
    <img src="../assets/orochi-network.png" alt="Orochi Network">
</p>

# Orochi Network: Building The Zero Knowledge Modular Data Availability Layer

## Challenges

**Data Integrity:** In a truly decentralized Web3, data integrity would be maintained through a distributed network of validators. Each node would be able to verify the correctness of data independently. But In many Web3 solutions today, some level of trust in third-party oracles is necessary. Oracles are essentially data feeds that bridge the gap between the blockchain and the real world. They provide external data to smart contracts, which can then be used to trigger actions based on real-world events.

**Data Availability:** Smart contracts execute within isolated environment, the smart contract runtime (e.g: EVM, WASM) on the blockchain. This isolation/virtualization protects them from malicious attacks but also restricts their ability to interact directly with external data sources, like those in the real world. And also as the number of users and transactions on a blockchain network increases, storing and accessing all data on-chain becomes difficult and expensive.

## Our Solutions

At the heart of Orochi Network lies **Zero-Knowledge Modular Data Availability Layer (ZK Modular DA Layer - zkMDAL)**. Our system leverages the power of Zero-Knowledge Proofs (ZKPs) to create a high-performance **Verifiable Data Pipeline** for zkApp (Zero-Knowledge Applications), dApp (Decentralized Applications) and smart contract platforms. ZKPs allow us to prove the validity of every steps of data processing without revealing any other information. This innovation allows zkApps, dApps and smart contract platform to access unlimited number of off-chain states securely and efficiently in a verifiable manner. Developers can then focus on building innovative applications, leaving the complexities of **Data Integrity** and **Data Availability** to zkMDAL. But what exactly is zkMDAL, and how does it solve Web3's problems?

### zkDatabase: Zero-Knowledge Modular Data Availability Layer

While traditional Data Availability Layers focus on storing and guaranteeing the accessibility of data, zkDatabase goes a step further. It acts as a **Verifiable Data Pipeline**, the first of its kind for Web3. This pipeline offers several key advantages:

- **Verifiable Data Integrity:** Unlike other Data Availability Layers which are combine of distributed storage and commitment scheme, zkDatabase ensures data integrity by employing ZKP along side with Distributed Storage and Commitment Schemes. These ZKPs guarantee that the information processed satisfies the constraints required in the database, hasn't been tampered with and the proof is succinct, hence it can be verified on other blockchains effectively.
- **Proof-of-Everything of Data:** We do not just focus on verifying the data itself. zkDatabase aims to generate proofs encompassing everything related to the data – its origin, storage location, and any transformations it undergoes. This comprehensive approach fosters unparalleled trust and accuracy.
- **ZK-Rollups for Interoperability:** Unlike some traditional Data Availability Layers that can not bundle queries, zkDatabase leverages ZK-rollups to perform rollup on data. This approach allows zkDatabase to bundle multiple database queries in one single succinct proof. This interoperability eliminates the need for centralized oracles and reduces fees to verify the database's proof.

In essence, zkDatabase transcends the limitations of existing Data Availability Layers. It provides a comprehensive Verifiable Data Pipeline, offering verifiable data integrity, proof-of-everything for enhanced trust, and interoperability through zk-rollups. This combination empowers developers to build secure and reliable Web3 applications with confidence.

### zkMemory: A Generic Memory Prover in Zero-Knowledge Proof

zkMemory is a key innovation within Orochi's zkMDAL. It represents a modular memory prover, designed to be agnostic to specific proof systems. This means zkMemory can potentially work with various zero-knowledge proof systems, offering greater flexibility and adaptability for Orochi's zkMDAL.

- **Proof-System Agnostic:** As mentioned earlier, zkMemory's modular design allows it to function with different proof systems. This enhances the overall adaptability of Orochi's zkMDAL.
- **Future-Proof Design:** A modular approach like zkMemory positions Orochi's zkMDAL to embrace advancements in zero-knowledge proof systems as they emerge.

zkMemory is not only a back-end of zkMDAL but also a powerful building block for creating secure and efficient Zero-Knowledge Virtual Machines (zkVMs). Its modular design allows developers to integrate zkMemory into their zkVM architecture. This component acts as a dedicated memory prover, handling the zk-proof generation for memory operations within the zkVM. By leveraging zkMemory's modularity, developers can design zkVM applications with customized instruction sets and architecture, enabling them to tailor the virtual machine to specific needs. This flexibility, coupled with zkMemory's potential efficiency gains in proof generation, paves the way for a new generation of zkVM applications within the Web3 landscape.

### Other Web3 Products as a Service

#### Orand: Unleashing Verifiable Randomness for Secure Web3 Applications

Orand tackles this challenge head-on by providing a mechanism for generating **Verifiable Randomness**. Here's how Orand makes a difference:

- **Eliminates Trusted Third Parties:** Unlike traditional methods, Orand eliminates the need for relying on a single entity to generate random numbers. This removes a potential point of failure and strengthens the overall security of the system.
- **Verifiable Randomness:** Orand doesn't not just generate random numbers; it also provides cryptographic proofs that the random output is computed correctly and hasn't not been tampered with. This verifiable aspect fosters trust and transparency in dApps that rely on randomness for fair play and security-critical operations.
- **Wide Range of Applications:** Verifiable randomness from Orand can be utilized across various Web3 applications. Imagine dApps for provably fair gaming, secure lotteries, or unpredictable elements within the metaverse – Orand empowers developers to build these applications with confidence in the integrity of the randomness used.

By providing a secure and verifiable source of randomness, Orand empowers developers to build robust and trustworthy Web3 applications. This fosters a more secure and transparent online ecosystem for everyone.

#### Orosign: The Secure Gateway to Your Web3 Assets

Orosign acts as the secure front-end for Orochi Network and blockchains, providing a user-friendly interface for interacting with the decentralized world. Here's what makes Orosign unique:

- **Secure Digital Asset Management:** Orosign functions as a secure wallet for your digital assets. It utilizes multi-signature/threshold-signature functionality, allowing for shared control over your assets. This means multiple parties need to approve transactions, adding an extra layer of security and reducing the risk of unauthorized access.
- **Enhanced Protection:** Orosign integrates seamlessly with core principle of Web3. This means that even within Orosign, critical operations can be verified cryptographically.

**Beyond a Simple Wallet:**

Orosign goes beyond the functionalities of a traditional crypto wallet. It acts as a secure gateway, empowering users to:

- **Interact with dApps:** Orosign allows users to seamlessly interact with decentralized applications (dApps). With verifiable computation integrated, users can have greater confidence in the validity and security of their interactions with these dApps.
- **Manage Identity in a Decentralized World:** Orosign can potentially evolve into a tool for managing your digital identity within the Web3 space. By leveraging ZKP, Orosign could provide a secure and verifiable way to prove ownership of credentials or control access to specific information.

**The Future of Orosign:**

As Orochi Network continues to develop, Orosign has the potential to become a comprehensive and secure platform for managing your digital assets and identity in the decentralized future. Orosign aims to empower users to navigate the Web3 landscape with confidence and control.

#### Orocle: Decentralized Oracle Service

In the realm of Web3, where smart contracts reign supreme, traditional data sources are often inaccessible due to the isolated nature of blockchains. Here's where Decentralized Oracle Services (DOs) step in to bridge the gap. Unlike their centralized counterparts, DOs eliminate the single point of failure by relying on a distributed network of nodes. These nodes work collaboratively to gather and verify real-world data, ensuring its accuracy and reliability. This empowers smart contracts to access the external data they need to function as intended, unlocking a wider range of possibilities within the decentralized world. DOs represent a crucial step towards achieving a truly trustless and secure Web3 ecosystem.

#### Orochimaru: Orochi Network Full-node client

Built upon the robust Substrate Framework, Orochimaru plays a critical role in ensuring network security and functionality. It achieves this by validating transactions, securing the network through participation in Orochi Consensus, an asynchronous Byzantine Fault Tolerance (aBFT) consensus mechanism. This advanced consensus protocol allows Orochimaru nodes to reach agreement on the state of the network even in the presence of potentially faulty nodes, regardless of any delays or message losses. In essence, Orochimaru acts as the backbone of Orochi Network, safeguarding its integrity and enabling smooth operation even under challenging conditions.

## The Future of Web3

Orochi Network's zkMDAL is a promising step towards a more secure, scalable, and user-friendly Web3. By leveraging the power of Zero-Knowledge Proofs, zkMDAL offers solutions to some of the most pressing challenges facing the decentralized future of the internet. As zkMDAL continues to evolve, it has the potential to be a game-changer for Web3, ushering in a new era of innovation and user adoption.
In essence, our suite of products, anchored by the innovative zkMDAL, lays the groundwork for a future web built on secure, scalable, and user-friendly decentralized applications. By addressing the limitations of current dApps, Orochi Network has the potential to unlock the true potential of Web3, paving the way for a more decentralized and empowering online experience for everyone. The promise of Orochi Network has been recognized by leading organizations within the blockchain space. Orochi Network is a grantee of the **Ethereum Foundation**, **Web3 Foundation**, **Mina Protocol**, and **Aleo**. This recognition underscores the potential of our technology to shape the future of Web3.

## Orochi ❤️ Open Source

All projects are open-sourced and public. We build everything for common good.

- Our scientific paper about a new proof-system: [RAMenPaSTA: Parallelizable Scalable Transparent Arguments of Knowledge for RAM Programs](https://eprint.iacr.org/2024/336)
- Our construction in Distributed ECVRF - [ORAND - A fast, publicly verifiable, scalable decentralized random number generator for blockchain-based applications](https://docsend.com/view/5y7rc5cww2juudzn)
- Our proposal to improve security of Smart Contracts - [ERC-6366: Permission Token](https://eips.ethereum.org/EIPS/eip-6366)
- Our proposal to improve permission and role handling - [ERC-6617: Bit Based Permission](https://eips.ethereum.org/EIPS/eip-6617)
