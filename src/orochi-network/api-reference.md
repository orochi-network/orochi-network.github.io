# API Reference

## Initial

We assume that you have an initialized instance of Orand.

```ts
import { orand } from "@orochi-network/sdk";

const orandInstance = new orand.Orand({
  url: "https://orand-test-service.orochi.network/",
  user: "YOUR_REGISTERED_USERNAME",
  secretKey: "YOUR_REGISTERED_HMAC_SECRET",
  consumerAddress: "YOUR_APPLICATION_SMART_CONTRACT_ADDRESS",
  chaiId: 1,
});
```

**Note:**

To prevent replay attack, we're required you to input `consumerAddress`. It's a smart contract address that will consume the randomness. Each `consumerAddress` will have different epoch target, you only able to to submit the epoch which has the matched epoch with target epoch on `OrandProviderV1` smart contract.

Some results will be return in `BigInt` due to new update from `ethers.js`.

### .getPublicEpoch()

```ts
await orandInstance.getPublicEpoch(0);
```

Allowed you to get a list of public epochs at the starting `epoch`, it will return an array of `IOrandEpoch`. The result will be limited to 20 records to prevent Denial of Service.

Public epoch has `receiverAddress` is `0x0000000000000000000000000000000000000000`, submit public epoch to `OrandProviderV1` won't have any affect since the consumer address is `0x0000000000000000000000000000000000000000`.

**Result:**

```txt
[
  {
    epoch: 0,
    alpha: '195ccc785b997579566fcac4367ee662fffba11f0d7c67d6df4cbbd318251fdd',
    gamma: '59c6bd805ff9b9e21fe7888d1a3a2672e02f950c58814e84d7537ada90c31a7bcf64410d775ec3de37280688c1021a23a32098b590abc3b73f630378fc42c053',
    c: 'b50167c3019820270b3708d3d586d212b4289d9b83d6b0f8350432045b8e9c4d',
    s: 'cc13dc051aeb7cbdcc3bb4e8bfc3e92b1ab254899d503ee20813f7f77c107f7d',
    y: '0a22386520c728455c84b21830ae0a21d56752dd27fe13ae129b0a04f599c0b2',
    witnessAddress: '0c2971ca2f70ca924d3bfb08b94677a6cf9ab068',
    witnessGamma: '8205aee30d8d3c8b9504c467f03b790ec7d7106125e45a8ee9e784f9670341d4429115d24958b64611f17c8d3a748951d27ce2a1c696da80c5edb56c78cf678a',
    witnessHash: '665a9bb45478fa3391bf6308f6752ddc152288d53e4ed393230746ff74504817d71850c87edd776494f053aaf5b3e39e34b5cb05fc468e8d29553faf0e38ea62',
    inverseZ: '60d0c2324755fd8f091c8fa690cd43899c94f0519958f8b85363d804d5f382e2',
    signatureProof: '14d0bdf7f9457321871bc68490d23f04e8d5b394f2b43a7534f2bc01fea694d576ec3c24667d31475cf31747fccc3e79eb70823f852a420dfbf191e3a863b6de1b00000000000000000000000000000000000000000000000000000000000000000a22386520c728455c84b21830ae0a21d56752dd27fe13ae129b0a04f599c0b2',
    createdDate: '2023-03-10 06:40:56'
  },
  {
    epoch: 1,
    alpha: '0a22386520c728455c84b21830ae0a21d56752dd27fe13ae129b0a04f599c0b2',
    gamma: '00e995ee0917b73aea5cbcee6b5583f20e67c80666749359a8c97aa25fea97f88c4fa15d5feffd7815edc9b867c99891f26407cd53c3ad1b20fcb81c913a35a9',
    c: '57f2d6461f1d05f7db5aca504c2b022e23854eeb638a47f56f32ca95730cf17b',
    s: '01b4315ddb662d6bf326b64739777f47dc9a4e1e9362f380a98f9f5a41013951',
    y: '08a5571c2c093c48c16c52dc0ec131a87073a5ff3ae75de354a5f7b66c5b5d8b',
    witnessAddress: 'cab1927ebecfb748f021deed4e0f54acee27dd62',
    witnessGamma: '896da8ba810f6c0412414773fa1012244f3fb0cb086702f78c4420622420ab3f71d7f20e9648c2cce7a65c57ae1f689555d15c357990e639be54a27aaa34e395',
    witnessHash: 'ea1af5c829afdc666c448e1f2aedc4e1fe024bd5a321303cc1f71bacd94558ff23035dbf54c820ec18e6fa371ec96a5f9f0d86493e1a3b08d2be767c3b389c02',
    inverseZ: 'ccc6ef8b4d4deee402c832cc85cc7ff12610dda57aa8a5ced90a05747f240d37',
    signatureProof: '3386c6ba69dd850fa799af07544332f8d45b431ba65f5db34c79345839c921764f89f53a8cb2652920c9495a76804a1b2aa11c01ade58b19c78ae6621f60feaa1b000000000000000000000001000000000000000000000000000000000000000008a5571c2c093c48c16c52dc0ec131a87073a5ff3ae75de354a5f7b66c5b5d8b',
    createdDate: '2023-03-10 06:40:58'
  }
]
```

### .newPrivateEpoch()

```ts
await orandInstance.newPrivateEpoch();
```

Allowed you to generate a new private epoch that related to `consumerAddress`. The `consumerAddress` need to be registered on Orand V1 service.

**Result:**

```txt
{
  epoch: 3,
  alpha: 'c9677c0884f380b1facece540fb2674590c6b004207c72d3fa3f99c6699e2401',
  gamma: '8a3059cec8687c2d9d7048098a8484b0ebb8839d6589be070affac5a7763dd8e73a316231302534ea834e897835f610d61bc4dd8a2b75b71be35414b2fb2a2ea',
  c: '9345c77ac9c7c1ba6d084ccf6997e2fcf623dc9cdc72e42a91855b1c8cc1c5fa',
  s: '8457bc202e735627d5a27133744bf3bb0dd72fb5e85ce826d0716c107fd44430',
  y: '147c78180c05d041a9e8b3bb11cf59f1c871019db940532bd38df22f8194f28c',
  witnessAddress: '6dfb0007085713f2621380670e8578eb83df552b',
  witnessGamma: 'a191f86cff2b88e06b38c64e4d2ddfb3c0673b32c7e229bd46f6a2262a962f700765aba72c8d225133874d0f26327a6851a67946b2deb1d7c722d4836d4cd1f4',
  witnessHash: '0142eeaf5d41dad6a3b2f42f0a06ae8b17b47084c78bcd86f44693a264a9defb72a9b3ee2a3466810bb69e6360ca02f16b32f5f241ae7089651bc8f6a1f580c4',
  inverseZ: '77024d6c015a9b0cbe758246843d7d8712115abaac53f761a6e1594d240a8a02',
  signatureProof: '6f7e2c4eef8a3bbc0699971ab2d80693702356f1873d45b20829b50873943e8975f6f978d1daafc10c2c7ecda8bf3dda823485cfa7d17740398852423188f54c1b000000000000000000000003f16f07cfd6e9ac06925fcf68dd0b450f4131989d147c78180c05d041a9e8b3bb11cf59f1c871019db940532bd38df22f8194f28c',
  createdDate: '2023-03-10 06:56:08'
}
```

### .getPrivateEpoch()

```ts
await orandInstance.getPrivateEpoch(3);
```

Allowed you to get a list of private epoch the starting `epoch`, it will return an array of `IOrandEpoch`. The result will be limited to 20 records to prevent Denial of Service. These result will be bound to `receiverAddress`.

**Result:**

```txt
[
  {
    epoch: 3,
    alpha: 'c9677c0884f380b1facece540fb2674590c6b004207c72d3fa3f99c6699e2401',
    gamma: '8a3059cec8687c2d9d7048098a8484b0ebb8839d6589be070affac5a7763dd8e73a316231302534ea834e897835f610d61bc4dd8a2b75b71be35414b2fb2a2ea',
    c: '9345c77ac9c7c1ba6d084ccf6997e2fcf623dc9cdc72e42a91855b1c8cc1c5fa',
    s: '8457bc202e735627d5a27133744bf3bb0dd72fb5e85ce826d0716c107fd44430',
    y: '147c78180c05d041a9e8b3bb11cf59f1c871019db940532bd38df22f8194f28c',
    witnessAddress: '6dfb0007085713f2621380670e8578eb83df552b',
    witnessGamma: 'a191f86cff2b88e06b38c64e4d2ddfb3c0673b32c7e229bd46f6a2262a962f700765aba72c8d225133874d0f26327a6851a67946b2deb1d7c722d4836d4cd1f4',
    witnessHash: '0142eeaf5d41dad6a3b2f42f0a06ae8b17b47084c78bcd86f44693a264a9defb72a9b3ee2a3466810bb69e6360ca02f16b32f5f241ae7089651bc8f6a1f580c4',
    inverseZ: '77024d6c015a9b0cbe758246843d7d8712115abaac53f761a6e1594d240a8a02',
    signatureProof: '6f7e2c4eef8a3bbc0699971ab2d80693702356f1873d45b20829b50873943e8975f6f978d1daafc10c2c7ecda8bf3dda823485cfa7d17740398852423188f54c1b000000000000000000000003f16f07cfd6e9ac06925fcf68dd0b450f4131989d147c78180c05d041a9e8b3bb11cf59f1c871019db940532bd38df22f8194f28c',
    createdDate: '2023-03-10 06:56:08'
  }
]
```

### .verifyECDSAProof()

```ts
await orandInstance.verifyECDSAProof(epochData);
```

Allowed you verify the ECDSA proof of any given epoch.

**Result:**

```txt
{
  signer: '0x7e9e03a453867a7046B0277f6cD72E1B59f67a0e',
  receiverEpoch: 2n,
  receiverAddress: '0xF16F07cfd6e9Ac06925FCf68dD0b450f4131989D',
  y: 91097723859136686723473270573409338368251757405505259439091937259103551628289n
}
```

- `signer` MUST be equal to `await orandInstance.getOperatorAddress()` to make sure the ECDSA proof is valid.
- `receiverEpoch` the required epoch number that need to be submit in the next publication.
- `receiverAddress` MUST be equal to predefine `receiverAddress` in the initial stage
- `y` Result of the current epoch

### .getActiveChain()

```ts
await orandInstance.getActiveChain();
```

Allowed you read the data of current active chain.

**Result:**

```txt
{
  url: 'https://data-seed-prebsc-1-s2.binance.org:8545',
  providerAddress: '0x75C0e60Ca5771dd58627ac8c215661d0261D5D76'
}
```

- `url` is the JSON-PRC provider, if you don't want you can use another JSON-RPC provider
- `providerAddress` is `OrandProviderV1` address on active chain

### .getOperatorAddress()

```ts
await orandInstance.getOperatorAddress();
```

Get the operator's address that going to sign the ECDSA Proof.

**Result:**

```txt
0x7e9e03a453867a7046B0277f6cD72E1B59f67a0e
```

### .getReceiverAlpha()

```ts
await orandInstance.getReceiverAlpha(
  "0xf16f07cfd6e9ac06925fcf68dd0b450f4131989d"
);
```

Get current `alpha` of receiver, current `alpha` is previous the result of previous epoch. The next epoch MUST have the same `alpha` with current receiver's `alpha`.

**Result:**

```txt
114997148547855332310731174935020155906209462858493962385407246111280193662921n
```

### .getReceiverEpoch()

```ts
await orandInstance.getReceiverEpoch(
  "0xf16f07cfd6e9ac06925fcf68dd0b450f4131989d"
);
```

Get current `epoch` of receiver, this value will be increased after a new epoch data submit to `OrandProviderV1`. This value MUST be equal to the epoch number of the submitting epoch.

**Result:**

```txt
1n
```

### .verifyECVRFProof()

```ts
await orandInstance.verifyECVRFProof(epochData);
```

Allowed you to verify the correctness of any epoch, it will return `true` if the given epoch is valid otherwise it returns `false`.

**Result:**

```txt
true
```

**Note:** We might provide a new feature to reroll current epoch, in case the result failed to be verified on-chain. This reroll feature only change the witness but the result will be the same.
