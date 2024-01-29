# Orand Code Integration

### Randomness Feeding Process

Your application can request the verifiable randomness directly from Orand. Then you can publish the randomness yourself by calling the method `publish()` (`genesis()` for the genesis epoch) on smart contract `OrandProviderV2`. Higher tier of service establishes the ability to submit the randomness from Orand service but the Orochi Network will cover the cost for each randomness.

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
import { Orand } from "@orochi-network/sdk";

(async () => {
  // Create an instance of Orand
  let orandInstance = await Orand.fromRPC(
    {
      user: "YOUR_REGISTERED_USERNAME",
      secretKey: "YOUR_REGISTERED_SECRET",
      url: "ORAND_SERVICE_URL",
      consumerAddress: "YOUR_CONSUMER_ADDRESS",
    },
    "NETWORK_RPC_URL"
  );

  const newEpoch = await orandInstance.newPrivateEpoch();
  console.log(newEpoch);
})();
```

- **ORAND_SERVICE_URL:** We will provide you the service URL
- **YOUR_REGISTERED_USERNAME:** The username that you registered with Orand service, _e.g: "Navi"_
- **YOUR_REGISTERED_SECRET:** The random secret key used to authenticate yourself, _for instance, 6b1ab607287f5379db8b70bb7515feaa_
- **YOUR_CONSUMER_ADDRESS:** The consumer smart contract address _for instance, "0x66681298BBbdf30a0B3Ec98cAbf41AA7669dc201"_
- **NETWORK_RPC_URL:** JSON RPC endpoint of network, All configurations will be load base on `ChainId`

**Note:** `ChainId` is a predefined value, you can check [the chain list here](https://chainlist.org/).

### Submit The Randomness To Your Smart Contract

```ts
const wallet = Wallet.fromPhrase(
  "YOUR_WALLET_PASSPHRASE"
  orandInstance.rpcProvider
);

const newEpoch = await orandInstance.newPrivateEpoch();
console.log(newEpoch);
console.log(await orandInstance.publish(newEpoch, wallet));
```

Don't mind to contact us on Telegram if you need support [https://t.me/OrochiNetwork](https://t.me/OrochiNetwork).
