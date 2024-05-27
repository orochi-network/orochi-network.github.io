# Orand V3

Orand project was built based on Elliptic Curve Verifiable Random Function (ECVRF). It is deterministic, verifiable and secured based on assumptions from elliptic curves. Administrators of Orochi Network are unable to manipulate the results.

To optimize operation costs and improve security we provided following features:

- **Verifiable:** An Orand's epoch can be verified independently outside our system or can be verified by smart contracts.
- **Self and Delegated Submission:** Orand project have flexibility in proof submission, we just generate valid ECVRF proof and you can decide how to submit them:
  - **Self Submission:** You can request from your back-end to Orand service and submit the randomness to your smart contract.
  - **Delegation Submission:** You can delegate the submission process to Orochi Network by transfer token to our operator, so the feeding process will be performed automatically.
  - **Request Submission:** Instead of request to Orand service, you can request randomness via Oracle contract.
- **Batching:** We allow you to set the batching limit for one epoch, e.g., we can batch `100` randomness for one single epoch which makes the cost be reduced significantly.

## Deployed Platform

Orand V3 was deployed on following smart contract platform.

### Mainnet

| Network Name | Address                                                                                                              |
| ------------ | -------------------------------------------------------------------------------------------------------------------- |
| BNB Chain    | [0x4DB03CC56421A81EC1398279913d8D55E62f5328](https://bscscan.com/address/0x4DB03CC56421A81EC1398279913d8D55E62f5328) |

### Testnet

| Network Name             | Address                                                                                                                                        |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Ancient8 Testnet         | [0xD7a2643c1d9C3E6069f90DbAabd9D58825C7A2b9](https://scanv2-testnet.ancient8.gg/address/0xD7a2643c1d9C3E6069f90DbAabd9D58825C7A2b9)            |
| Unicorn Ultra Nebulas    | [0x3eAF9da360dA944105599cdB7833712346af6DF1](https://testnet.u2uscan.xyz/address/0x3eAF9da360dA944105599cdB7833712346af6DF1)                   |
| Sei Devnet               | [0x2182fD816A3fCeCDeD390452524cfA8f21142A88](https://seitrace.com/address/0x2182fD816A3fCeCDeD390452524cfA8f21142A88)                          |
| Saakuru TestNet          | [0xA117B152a7bB8fAa9920e2C51B1Fb95452D2936f](https://explorer.testnet.oasys.games/address/0xA117B152a7bB8fAa9920e2C51B1Fb95452D2936f)          |
| Zircuit TestNet          | [0x1Ae52e0Ef3f78F015AEF13AbCB4776333fb073C2](https://explorer.zircuit.com/address/0x1Ae52e0Ef3f78F015AEF13AbCB4776333fb073C2)                  |
| ZKFair TestNet           | [0xE733CaC2c60effe593587E595Ef6F98dA44a7cf4](https://testnet-scan.zkfair.io/address/0xE733CaC2c60effe593587E595Ef6F98dA44a7cf4)                |
| X Layer TestNet          | [0x50C72F5bea0757c8052daa6402568d4bbf2336Fb](https://www.okx.com/web3/explorer/xlayer-test/address/0x50C72F5bea0757c8052daa6402568d4bbf2336Fb) |
| ZKLink Nova              | [0xf9338096bb1bCdBDB83E5a237F198A60A48395a2](https://sepolia.explorer.zklink.io/address/0xf9338096bb1bCdBDB83E5a237F198A60A48395a2)            |
| Bsc Testnet              | [0xA9aA047CaA5C24A4fC390F3887C98b20bc4e6556](https://testnet.bscscan.com/address/0xA9aA047CaA5C24A4fC390F3887C98b20bc4e6556)                   |
| Arbitrum Sepolia Testnet | [0x398027eA740de745FE4Be768e6e5744A6C58514C](https://sepolia.arbiscan.io/address/0x398027eA740de745FE4Be768e6e5744A6C58514C)                   |
| Moonbase Alpha TestNet   | [0xb368A56355707bC23321C85a9f2f1773B09a5a22](https://moonbase.moonscan.io/address/0xb368A56355707bC23321C85a9f2f1773B09a5a22)                  |

## Self Submission

User will request the verifiable randomness from Orand service, they can submit the randomness themselves and control gas consumption. You must submit epoch by sequence and starting epoch or genesis must be epoch `0`.

```mermaid
%%{init: {'theme':'base'}}%%
sequenceDiagram
    Game Backend->>+Orand: Request randomness
    Orand->>-Game Backend: VRF Epoch
    Game Backend->>+Orand Contract: Publish VRF Epoch
    Orand Contract->>-Game Backend: Transaction Receipt
```

## Delegated Submission

User will delegate the submission process to Orochi Network, first they need to deposit native token to operator address that provided by Orochi Network.

```mermaid
%%{init: {'theme':'base'}}%%
sequenceDiagram
    Game Backend->>+Orand: Request randomness
    Orand->>+Orand Contract: Publish VRF Epoch
    Orand Contract->>-Orand: Transaction Receipt
    Orand->>-Game Backend: VRF Epoch + Tx Receipt
```

## Request Submission

dApp will request to Orochi Network's oracle contract for the randomness, Orand service will fulfill this request and submit the randomness to Orand provider contract.

```mermaid
%%{init: {'theme':'base'}}%%
sequenceDiagram
    Game Frontend->>+Oracle: Request randomness
    Orand->>+Orand: Repeating Polling Request
    Orand->>-Orand Contract: Fulfil Request
    Oracle->>Game Frontend: Tx Receipt
```

## Orand V3

Orand V3 will focus on utilizing Multi Party Computation (MPC) to secure the randomness generation, allowing the whole system to act as one random oracle. It makes the process more dispersed. In this stage, we boot up **Chaos Theory Alliance** to preventing predictability. Everything is built up toward to the vision of **Decentralized Random Number Generator**. If you believe in the vision of **Decentralized Random Number Generator**, please send drop us an email to ([contact@orochi.network](contact@orochi.network)) in order to participate in **Chaos Theory Alliance**.
