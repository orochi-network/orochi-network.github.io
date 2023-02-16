# Orochi Network SDK

Orochi Network SDK provides client-side tools that allow you to interact with the whole Orochi Network ecosystem. We supported browser and Node.js at the first place.

## Installation

You could install Orochi Network sdk by running:

```bash
npm install @orochi-network/sdk
```

Please take note that `@orochi-network/sdk` requires `es2018` to work as expected.

## Usage

First you might need to import `@orochi-network/sdk` to your project

```ts
import { orand, IOrandEpoch } from "@orochi-network/sdk";
```

After you import the sdk, you can use our sdk in your project.

```ts
const orandInstance = new orand.Orand({
  url: "https://orand-test-service.orochi.network/",
  user: "YOUR_REGISTERED_USERNAME",
  secretKey: "YOUR_REGISTERED_HMAC_SECRET",
  consumerAddress: "YOUR_APPLICATION_SMART_CONTRACT_ADDRESS",
});
```

In the example above, we init an instance of `Orand` which provides verifiable randomness based on [ECVRF](../ecvrf/verifiable_random_function.md).

To learn about Orand integration please check [next section](./contract-integration.md).
