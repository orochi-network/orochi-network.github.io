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

| Network Name            | Address                                                                                                                                   |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Ancient8 Mainnet        | [0x970FD01B4427Ec1b48a8105aee7A3fc4E70E5FE5](https://scan.ancient8.gg/address/0x970FD01B4427Ec1b48a8105aee7A3fc4E70E5FE5)                 |
| U2U Solaris Mainnet     | [0x635eB277805AFADDd720B873dfE87F8fc01b24e4](https://u2uscan.xyz/address/0x635eB277805AFADDd720B873dfE87F8fc01b24e4)                      |
| BNB Chain               | [0x4DB03CC56421A81EC1398279913d8D55E62f5328](https://bscscan.com/address/0x4DB03CC56421A81EC1398279913d8D55E62f5328)                      |
| X Layer Mainnet         | [0x6254c96c3d96653FCD4A7133Ff138F97656522B7](https://www.okx.com/web3/explorer/xlayer/address/0x6254c96c3d96653FCD4A7133Ff138F97656522B7) |
| Saakuru (L2)            | [0x3CEA68A48c01Ff0759C3df54324b4E3B6F284303](https://explorer.saakuru.network/address/0x3CEA68A48c01Ff0759C3df54324b4E3B6F284303)         |
| Sei Mainnet             | [0x6f24d4da6b9bBdbE4d5F2e3C663c8F1C617bdf13](https://seitrace.com/address/0x6f24d4da6b9bBdbE4d5F2e3C663c8F1C617bdf13?chain=pacific-1)     |
| Manta Pacific L2 Rollup | [0x2578F22C7488E08B6D8ED3Bb042b70a3444BF111](https://pacific-explorer.manta.network/address/0x2578F22C7488E08B6D8ED3Bb042b70a3444BF111)   |
| ZKsync Era Mainnet      | [0x29ca057761732432bcA26109A291291a365755b4](https://explorer.zksync.io/address/0x29ca057761732432bcA26109A291291a365755b4)               |
| Scroll Mainnet          | [0x4e7dc620996e58A2BCba128c6b0C7e9e2De103dC](https://scrollscan.com/address/0x4e7dc620996e58A2BCba128c6b0C7e9e2De103dC)                   |
| Zircuit Mainnet         | [0x99C2b1CcA077ceA2c84Fdc160aAc1A7f64ecf6be](https://explorer.zircuit.com/address/0x99C2b1CcA077ceA2c84Fdc160aAc1A7f64ecf6be)             |
| Phoenix Mainnet         | [0x0b226bf21B67A691775F9eDbc529D244FC79ab49](https://phoenix.lightlink.io/address/0x0b226bf21B67A691775F9eDbc529D244FC79ab49)             |
| Etherlink Mainnet       | [0xbaD95Bd46411785c1189a8dd06C1C354DE805170](https://explorer.etherlink.com/address/0xbaD95Bd46411785c1189a8dd06C1C354DE805170)           |
| 5ireChain Mainnet       | [0x53d56D3da563678f92CD87622fa72C1ae5c0876B](https://5irescan.io/contract/evm/0x53d56D3da563678f92CD87622fa72C1ae5c0876B)                 |

### Testnet

| Network Name                  | Address                                                                                                                                                 |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ancient8 Testnet              | [0xD7a2643c1d9C3E6069f90DbAabd9D58825C7A2b9](https://scanv2-testnet.ancient8.gg/address/0xD7a2643c1d9C3E6069f90DbAabd9D58825C7A2b9)                     |
| Unicorn Ultra Nebulas         | [0x3eAF9da360dA944105599cdB7833712346af6DF1](https://testnet.u2uscan.xyz/address/0x3eAF9da360dA944105599cdB7833712346af6DF1)                            |
| Sei Devnet                    | [0x2182fD816A3fCeCDeD390452524cfA8f21142A88](https://seitrace.com/address/0x2182fD816A3fCeCDeD390452524cfA8f21142A88?chain=arctic-1)                    |
| Saakuru Testnet               | [0xA117B152a7bB8fAa9920e2C51B1Fb95452D2936f](https://explorer.testnet.oasys.games/address/0xA117B152a7bB8fAa9920e2C51B1Fb95452D2936f)                   |
| Zircuit Testnet               | [0x1Ae52e0Ef3f78F015AEF13AbCB4776333fb073C2](https://explorer.testnet.zircuit.com/address/0x1Ae52e0Ef3f78F015AEF13AbCB4776333fb073C2)                   |
| ZKFair Testnet                | [0xE733CaC2c60effe593587E595Ef6F98dA44a7cf4](https://testnet-scan.zkfair.io/address/0xE733CaC2c60effe593587E595Ef6F98dA44a7cf4)                         |
| X Layer Testnet               | [0x50C72F5bea0757c8052daa6402568d4bbf2336Fb](https://www.okx.com/web3/explorer/xlayer-test/address/0x50C72F5bea0757c8052daa6402568d4bbf2336Fb)          |
| ZKLink Nova                   | [0xf9338096bb1bCdBDB83E5a237F198A60A48395a2](https://sepolia.explorer.zklink.io/address/0xf9338096bb1bCdBDB83E5a237F198A60A48395a2)                     |
| BNB Chain Testnet             | [0xA9aA047CaA5C24A4fC390F3887C98b20bc4e6556](https://testnet.bscscan.com/address/0xA9aA047CaA5C24A4fC390F3887C98b20bc4e6556)                            |
| Arbitrum Sepolia Testnet      | [0x398027eA740de745FE4Be768e6e5744A6C58514C](https://sepolia.arbiscan.io/address/0x398027eA740de745FE4Be768e6e5744A6C58514C)                            |
| Moonbase Alpha Testnet        | [0xb368A56355707bC23321C85a9f2f1773B09a5a22](https://moonbase.moonscan.io/address/0xb368A56355707bC23321C85a9f2f1773B09a5a22)                           |
| Manta Pacific Sepolia Testnet | [0x060b6352406f6a22a5BAfCB372EDa87a3B077D23](https://pacific-explorer.sepolia-testnet.manta.network/address/0x060b6352406f6a22a5BAfCB372EDa87a3B077D23) |
| Kroma Sepolia Testnet         | [0xabD720eaf47339150E0d0dF379307a17A68092Ac](https://sepolia.etherscan.io/address/0xabD720eaf47339150E0d0dF379307a17A68092Ac)                           |
| Fantom Testnet                | [0xc3A7a16304D08002c27441e9e2Cb6366E97862B2](https://testnet.ftmscan.com/address/0xc3A7a16304D08002c27441e9e2Cb6366E97862B2)                            |
| Base Sepolia Testnet          | [0x6240Cb830Ab06cd872827b92D6E9Fe9cA8Eac432](https://base-sepolia.blockscout.com/address/0x6240Cb830Ab06cd872827b92D6E9Fe9cA8Eac432)                    |
| Optimism Sepolia Testnet      | [0x518fF0Ad9549BbB38000459C6F70bc78718cC0B1](https://sepolia-optimistic.etherscan.io/address/0x518fF0Ad9549BbB38000459C6F70bc78718cC0B1)                |
| Wanchain Testnet              | [0xD682B35eB08cC3cb74F690f6B8fFAE087625a158](https://testnet.wanscan.org/address/0xD682B35eB08cC3cb74F690f6B8fFAE087625a158)                            |
| Scroll Sepolia Testnet        | [0x25fa439D1030540FCd9B2E67F0e8A704ad078226](https://sepolia.scrollscan.dev/address/0x25fa439D1030540FCd9B2E67F0e8A704ad078226)                         |
| Morph Holesky Testnet         | [0x6d660F6e9bC72709c96448dEe00D0c08cfD77577](https://explorer-holesky.morphl2.io/address/0x6d660F6e9bC72709c96448dEe00D0c08cfD77577)                    |
| LightLink Pegasus Testnet     | [0x591749260B0dEc690976ce1dDFbd71694578b99e](https://pegasus.lightlink.io/address/0x591749260B0dEc690976ce1dDFbd71694578b99e)                           |
| Metis Sepolia Testnet         | [0xD2Aaf6e041596431D7df35aC95e7BA74735266cD](https://sepolia-explorer.metisdevops.link/address/0xD2Aaf6e041596431D7df35aC95e7BA74735266cD)              |
| B² Testnet                    | [0xE21Bc6E404A4133119A164352477C1439205Af27](https://testnet-explorer.bsquared.network/address/0xE21Bc6E404A4133119A164352477C1439205Af27)              |
| StratoVM Sepolia Testnet      | [0x2f685A31c47a0C9141EB5B2aF52cC2987a336B70](https://explorer.stratovm.io/address/0x2f685A31c47a0C9141EB5B2aF52cC2987a336B70)                           |
| Core Blockchain Testnet       | [0x396BBaA9eFfDFFa790e53b1a6b93AA161d65Cb75](https://scan.test.btcs.network/address/0x396BBaA9eFfDFFa790e53b1a6b93AA161d65Cb75)                         |
| Linea Testnet                 | [0x9FC1eE65D0485F2CFeeC5237D765DE5Af3f3f31c](https://sepolia.lineascan.build/address/0x9FC1eE65D0485F2CFeeC5237D765DE5Af3f3f31c)                        |
| Lumia Testnet                 | [0x5379742183018F098c160EeEf7DBe3187a23AA3A](https://testnet-explorer.lumia.org/address/0x5379742183018F098c160EeEf7DBe3187a23AA3A)                     |
| Etherlink Testnet             | [0x3e84eB29Df33832DE7c0E65BE3cC23F07Cb2fBdf](https://testnet.explorer.etherlink.com/address/0x3e84eB29Df33832DE7c0E65BE3cC23F07Cb2fBdf)                 |
| IoTeX Testnet                 | [0xB7a2e1ffa0Aaef491d381ABA2e07668f98B02C49](https://testnet.iotexscan.io/address/0xB7a2e1ffa0Aaef491d381ABA2e07668f98B02C49)                           |
| Chiado Testnet                | [0xE161A02Da435F36EC5EbfEAE84a0B81616Fa5F22](https://gnosis-chiado.blockscout.com/address/0xE161A02Da435F36EC5EbfEAE84a0B81616Fa5F22)                   |
| Minato Testnet                | [0x219Eed0551cB8BCf55b61e1CFB353f4Ad1F5bcF7](https://explorer-testnet.soneium.org/address/0x219Eed0551cB8BCf55b61e1CFB353f4Ad1F5bcF7)                   |
| AILayer Testnet               | [0x2210b037302Ea78CD424f484d70Db1a8E1118FfB](https://testnet-explorer.ailayer.xyz/address/0x2210b037302Ea78CD424f484d70Db1a8E1118FfB)                   |
| Polygon Amoy Testnet          | [0xD7a077a983986D9B948FF76C2151ba7e9ADA0b91](https://amoy.polygonscan.com/address/0xd7a077a983986d9b948ff76c2151ba7e9ada0b91)                           |
| Boba Sepolia Testnet          | [0xebEA88C5462f434174599dcE51D061695296543d](https://testnet.bobascan.com/address/0xebEA88C5462f434174599dcE51D061695296543d)                           |
| NERO Testnet                  | [0x38453d36e5152feca8a44cd020c3c607bb68a436](https://testnet.neroscan.io/address/0x38453d36e5152feca8a44cd020c3c607bb68a436)                            |
| Camp Network Testnet V2       | [0x0b43fc1C8fc373D3abDE9bb6bb8B7CfDc0Beb72A](https://camp-network-testnet.blockscout.com/address/0x0b43fc1C8fc373D3abDE9bb6bb8B7CfDc0Beb72A)            |
| 5ireChain Testnet             | [0x756887E2E85339822b97A943Fc750F7E5f354F26](https://testnet.5irescan.io/contract/evm/0x756887E2E85339822b97A943Fc750F7E5f354F26)                       |
| Matchain Testnet              | [0x59904eBEE55bea1B683eC34D68134585671BedB9](https://testnet.matchscan.io/address/0x59904eBEE55bea1B683eC34D68134585671BedB9)                           |

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
