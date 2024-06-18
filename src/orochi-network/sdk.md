# Orochi Network SDK

Orochi Network SDK provides client-side tools that allow you to interact with the whole Orochi Network ecosystem. We supported browser and Node.js at the first place.

## Installation

You can install Orochi Network sdk by running:

```bash
npm install @orochi-network/sdk
```

Please take note that `@orochi-network/sdk` requires `es2018` to work as expected.

## Usage

First you might need to import `@orochi-network/sdk` to your project

```ts
import { Orand } from "@orochi-network/sdk";
```

After you import the sdk, you can use our sdk in your project.

```ts
let orand = await Orand.fromRPC(
  {
    user: "YOUR_REGISTERED_USERNAME",
    secretKey: "YOUR_REGISTERED_SECRET",
    url: "https://orand-test.orochi.network",
    consumerAddress: "YOUR_CONSUMER_ADDRESS",
  },
  "https://rpcv2-testnet.ancient8.gg/"
);
```

Orochi Network is going to provide following data:

- `YOUR_REGISTERED_USERNAME`: Your identify in our system
- `YOUR_REGISTERED_SECRET`: Your HMAC-SHA256 secret key
- `YOUR_CONSUMER_ADDRESS`: Consumer address can be any valid EVM compatible address

**Note:** _for the mainnet `YOUR_CONSUMER_ADDRESS` need to be provided by you and should be a valid consumer contract that implied [IOrandConsumerV2](https://github.com/orochi-network/smart-contracts/blob/main/contracts/orand-v2/interfaces/IOrandConsumerV2.sol)_

In the example above, we initiated an instance of `Orand` which provides verifiable randomness based on [ECVRF](../ecvrf/verifiable_random_function.md).

To learn more about Orand integration please check [next section](./contract-integration.md).
