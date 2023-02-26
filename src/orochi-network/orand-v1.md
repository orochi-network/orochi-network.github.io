# Orand V1

Orand project was built based on Elliptic Curve Verifiable Random Function (ECVRF). It is deterministic, verifiable and secured based on assumptions from elliptic curves. Administrators of Orochi Network are unable to manipulate the results.

To optimize operation costs and improve security we provided following features:

- **Verifiable:** An Orand's epoch can be verified independently outside our system or can be verified by smart contracts.

- **Hybrid Proof System:** Our customers can choose between Fraud Proof or Validity Proof to feed the randomness.

- **Dual Proof:** An ECDSA proof will be combined with an ECVRF proof to secure the feeding process, allowing it to be done by any third party while still guaranteeing the result to be verifiable and immutable.

- **Batching:** We allow you to set the batching limit for one epoch, e.g., we can batch \\(1000\\) randomness for one single epoch which makes the cost be reduced significantly.

## Orand V2

Orand V2 will focus on utilizing Multi Party Computation (MPC) to secure the randomness generation, allowing the whole system to act as one random oracle. It makes the process more dispersed. In this stage, we boot up **Chaos Theory Alliance** to preventing predictability. Everything is built up toward to the vision of **Decentralized Random Number Generator**. If you believe in the vision of **Decentralized Random Number Generator**, please send drop us an email to ([contact@orochi.network](contact@orochi.network)) in order to participate in **Chaos Theory Alliance**.
