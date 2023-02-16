# Orand Code Integration

### Randomness Feeding Process

Your application can request the verifiable randomness directly from Orand. Then you can publish the randomness yourself by calling the method `publish()` on `OrandProviderV1` smart contract. Higher tier of service establish the ability so submit the randomness from Oran service but the deal will cover the cost for each randomness.

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

- **ORAND_SERVICE_URL:** We will provider you the service URL
- **ORAND_USER_NAME:** The username that you registered with Orand service, _e.g: "Navi"_
- **ORAND_USER_HMAC_SECRET:** The random secret key that used to authenticate yourself, _e.g: 6b1ab607287f5379db8b70bb7515feaa_
- **ORAND_USER_CONSUMER_ADDRESS**: The consumer smart contract address _e.g: "0x66681298BBbdf30a0B3Ec98cAbf41AA7669dc201"_

**Note:** `ChainId` is a predefined value, you could check [the chain list here](https://chainlist.org/).

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
      "0xF3455Bb39e8C9228f8701ECb5D5A177A77096593",
      abiOrandProviderV1
    )
  ))
)).connect(wallet);

// Convert Orand proof to smart contract proof
const [ecdsaProof, epoch] = orandInstance.transformProof(data);
console.log([ecdsaProof, epoch]);

// Publish the proof
await orandProviderV1.publish(ecdsaProof, epoch);
```

Don't might to contact us on Telegram if you need support [https://t.me/OrochiNetwork](https://t.me/OrochiNetwork).
