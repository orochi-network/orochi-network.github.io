# Orand Code Integration

### Randomness Feeding Process

Your application can request the verifiable randomness directly from Orand. Then you can publish the randomness yourself by calling the method `publishValidityProof()` on `OrandProviderV1` smart contract. Higher tier of service establishes the ability so submit the randomness from Orand service but the deal will cover the cost for each randomness.

```plain
┌───────────┐         ┌─────┐      ┌──────────────┐
│Application│         │Orand│      │Smart Contract│
└─────┬─────┘         └──┬──┘      └──────┬───────┘
      │                  │                │
      │Request Randomness│                │
      │─────────────────>│                │
      │                  │                │
      │                  │Get latest epoch│
      │                  │───────────────>│
      │                  │                │
      │                  │  Latest epoch  │
      │                  │<───────────────│
      │                  │                │
      │   ECVRF Proof    │                │
      │<─────────────────│                │
      │                  │                │
      │            ECVRF Proof            │
      │──────────────────────────────────>│
┌─────┴─────┐         ┌──┴──┐      ┌──────┴───────┐
│Application│         │Orand│      │Smart Contract│
└───────────┘         └─────┘      └──────────────┘
```

### Request Randomness From Orand

```ts
import {
  orand,
  IOrandEpoch,
  OrandProviderV1,
  abiOrandProviderV1,
} from "@orochi-network/sdk";
import { ethers } from "ethers";

(async () => {
  // Create an instance of Orand
  const orandInstance = new orand.Orand({
    url: "ORAND_SERVICE_URL",
    user: "ORAND_USER_NAME",
    secretKey: "ORAND_USER_HMAC_SECRET",
    consumerAddress: "ORAND_USER_CONSUMER_ADDRESS",
    chainId: 97,
  });
})();
```

- **ORAND_SERVICE_URL:** We will provide you the service URL
- **ORAND_USER_NAME:** The username that you registered with Orand service, _e.g: "Navi"_
- **ORAND_USER_HMAC_SECRET:** The random secret key used to authenticate yourself, _for instance, 6b1ab607287f5379db8b70bb7515feaa_
- **ORAND_USER_CONSUMER_ADDRESS**: The consumer smart contract address _for instance, "0x66681298BBbdf30a0B3Ec98cAbf41AA7669dc201"_

**Note:** `ChainId` is a predefined value, you can check [the chain list here](https://chainlist.org/).

### Submit The Randomness To Your Smart Contract

```ts
// Restore wallet from passphrase and connect it to JSON provider
let wallet = ethers.Wallet.fromPhrase(
  "YOUR_12_WORD_PASSPHRASE",
  new ethers.JsonRpcProvider("https://data-seed-prebsc-1-s1.binance.org:8545")
);

// Get epoch data from Orand
const data: IOrandEpoch = await orandInstance.newPrivateEpoch();

// Create an instance of OrandProviderV1
const orandProviderV1 = (<OrandProviderV1>(
  (<any>(
    new ethers.Contract(
      "0x75C0e60Ca5771dd58627ac8c215661d0261D5D76",
      abiOrandProviderV1
    )
  ))
)).connect(wallet);

// Convert Orand proof to smart contract proof
const [ecdsaProof, epoch] = orandInstance.transformProof(data);
console.log([ecdsaProof, epoch]);

// Publish the proof
await orandProviderV1.publishValidityProof(ecdsaProof, epoch);
```

Testing result of feeding `10` randomnesses to dice game contract [https://testnet.bscscan.com/tx/0x55c21d5c93c7ad314d25d28f49d7c6dc129bbc47a4df1c10b62dcdf579c385f2](https://testnet.bscscan.com/tx/0x55c21d5c93c7ad314d25d28f49d7c6dc129bbc47a4df1c10b62dcdf579c385f2).

Don't mind to contact us on Telegram if you need support [https://t.me/OrochiNetwork](https://t.me/OrochiNetwork).
