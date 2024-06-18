# API Reference

## Initial

We assume that you have an initialized instance of Orand.

```ts
import { Orand } from "@orochi-network/sdk";

let orand = await Orand.fromRPC(
  {
    user: "YOUR_REGISTERED_USERNAME",
    secretKey: "YOUR_REGISTERED_SECRET",
    url: "https://orand-test.orochi.network",
    consumerAddress: "YOUR_APPLICATION_SMART_CONTRACT_ADDRESS",
  },
  "https://rpcv2-testnet.ancient8.gg/"
);
```

**Properties:**

Orand instance will provide following properties:

- `rpcProvider`: The instance of `ethers.JsonRpcProvider` that will be used for static call and publish new epoch
- `orandProviderV2`: The instance of `ethers.Contract` that represent for `OrandProviderV2` smart contract. You can access all static method and interactive with provider smart contract of Orand after connect wallet.

**Note:**

To prevent replay attack, we're required you to input `consumerAddress`. It's a smart contract address that will consume the randomness. Each `consumerAddress` will have different epoch target, you only able to to submit the epoch which has the matched epoch with target epoch on `OrandProviderV2` smart contract.

Some results will be returned in `BigInt` due to new update from `ethers.js`.

### .newPrivateEpoch(): Promise\<OrandEpoch\>

```ts
await orandInstance.newPrivateEpoch();
```

Allowed you to generate a new private epoch that bound to `consumerAddress`. The `consumerAddress` need to be registered on Orand V2 service.

**Result:**

- `epoch`: Epoch number
- `alpha`: Seed of new epoch, `alpha` is the result of previous epoch
- `gamma`, `c`, `s`: Value of proof
- `y`: Result of new epoch
- `witnessAddress`, `witnessGamma`, `witnessHash`, `inverseZ`: Intermediate witness to prove given proof on smart contract.
- `signatureProof`: ECDSA proof that bound to new epoch.

**Output:**

```js
{
  epoch: 2,
  alpha: '6be9719911e75d8f9a889f13e7c8c703c859d8ff9c89dcf6aa8db219d5fc2d45',
  gamma: 'ef1408aa85d25157678b32423c467782a88fc9496a46d2adfef99019009eca3c278fa01773e51f6f72b27cf81e1eaf2e8a5855a7a705d87505734f2d1bf11e45',
  c: '0ab76c3ae301adae9fe4f4884c58e1c959f17d6ecbe8e0b1b9ed68353eebcf36',
  s: '2dc6a1202f37c91d373692d56754a9b8073ac298de3648080fa9483d8d5b2916',
  y: '5ba1272ccd60a4f450ded626d643611ef9419c7e7f4c9b742aca7688f7d3aa13',
  witnessAddress: '8495933d0449927c604dd6a855451feefa16013e',
  witnessGamma: '8797c2ef56f31ec34064de5a3df12d36b4db15dc0a97bd62d2687fff0e8acd46000dc045b4fa0d6fbc6325da0b80a2e5dd68623ecf03f5178e6adc89a442f530',
  witnessHash: '84b8797fad42b6851f9512be3222b10515edccd783f9a9505fe529e0aad5e96f3f6f21a38ead57ef1e61e61f95e1b5006f36a62b013db603da267c38303ce132',
  inverseZ: '336dd6497177819a37003c4b962c2196a41caf1649d9d397e13115dadbb1e373',
  signatureProof: '2c8d0c95decdadc5e7ebfb883956a66c3005de9beb95adc83da0d5882b0b7241086c833fde2898ea110fc86eb3d8a4fc2cd64d46c87a5966b3cb526475ced89e1b0000000000000000000000020f53d56bce68dc724687a1c89eea793fd67788811827aba56be6cb832e9eb3439919067afc8aec364399c6d155a6589cd67099b4',
  createdDate: '2024-01-29T05:17:33.536051'
}
```

### getPrivateEpoch(epoch?: number): Promise<OrandEpoch[]>

**Usage:**

```ts
await orandInstance.getPrivateEpoch(0);
```

Get recent private epochs.

**Params:**

- `epoch`: Allowed you to get a list of most recent public epochs that close to `epoch`, it will return an array of `OrandEpoch`. The result will be limited to 20 records to prevent Denial of Service. If `epoch` wasn't supplied it will return latest epochs.

**Result:**

It will return an array of `OrandEpoch`.

```js
[
  {
    epoch: 0,
    alpha: "e6d494bec47311a7d792ec1fdad02e8f79125ff515ceafe304e8dff27286c21a",
    gamma:
      "8be62be49e7ccb015e0df3e4326867f3e49187918a70b7350327f3d83d1e439022859a04162b2053f1dc7d3ce83c2be3ef1470fadfd58950d43b2031859bba64",
    c: "fa9c9ee4923b02b1aabaacdb8b60e3f421e5e6b8930e7ee45e4d53668d18b210",
    s: "3ed451751452a03443c46012c39ee68efedde1fd53d8ee81cb359cdae2d00b93",
    y: "be15ac8e5bd156fe925f3a9879100593f9cd8179713b04ac0ad2ded57fc6b7e8",
    witnessAddress: "7625d458345eba3a97b307315e517c341aa4eed4",
    witnessGamma:
      "9470d583d5f5c246b8be72f72ac4a905b6e89d21ebbb4362338fde837f45af0e680d0cd0885b06fde511d1eeed5078d7cef502a3ad9dd7c5bf39eacb554f0a6c",
    witnessHash:
      "d712ca266f9b43346aec040e61ec2be1ecf6beb9fe5276cc1e15286d37a635b919c124d8615cff81a0a8e81bf6e948e5c2b4678efc37c3febfbc71e4a0b106c6",
    inverseZ:
      "18537553a604b09da948eaf7a5b2aee9833f649556e6c891ddc629d380d04337",
    signatureProof:
      "1351bc757f1b3579afb04c60ef44b51ca9ec89da6141366bd030a3672b4928925aab4b31b4660fa3437d1ae55602c0474d9e5f0985d1a71c8b222670b3d9f7991b0000000000000000000000000f53d56bce68dc724687a1c89eea793fd677888151596c361cefc7f4781008cf9f0d044fcb2607515896666e20ab1eae18e9a63d",
    createdDate: "2024-01-28T07:16:59.853007",
  },
];
```

### verifyEpoch(epochECVRFProof: OrandEpoch): Promise\<VerifyEpochProofResult\>

**Usage:**

```ts
await orandInstance.verifyEpoch(epoch);
```

Validate an epoch is valid or not, it also check current epoch link status.

**Params:**

- `epoch`: A epoch of Orand, it can obtain by `getPrivateEpoch()`, `newPrivateEpoch()`

**Result:**

- `ecdsaProof`: Decomposed ECDSA proof
  - `signer`: Signer address of given signature
  - `receiverAddress`: Bound address to proof
  - `receiverEpoch`: Bound epoch to proof
  - `ecvrfProofDigest`: Digest of proof in `BigInt`, it can be convert to hex by `toString(16)`
- `currentEpochNumber`: Current epoch of given receiver
- `isEpochLinked`: Is this epoch linked to on-chain state
- `isValidDualProof`: Validity of dual proof
- `currentEpochResult`: On-chain epoch result, it will equal to `0n` if there is no genesis
- `verifiedEpochResult`: Result of verifying epoch

**Output:**

```js
{
  ecdsaProof: {
    signer: '0xED6A792F694b7a52E7cf4b7f02dAa41a7c92f362',
    receiverAddress: '0x3fc4344b63fb1AB35a406Cb90ca7310EC8687585',
    receiverEpoch: 0n,
    ecvrfProofDigest: 50338440222419549417414658096744085875859237350056663794916275947847078497254n
  },
  currentEpochNumber: 0n,
  isEpochLinked: false,
  isValidDualProof: true,
  currentEpochResult: 0n,
  verifiedEpochResult: 26822046345614045141848002846382424881390702704278990878694727729734354152825n
}
```

### publish(proof: OrandEpoch, wallet: ContractRunner): Promise\<ContractTransactionResponse\>

**Usage:**

```ts
const wallet = Wallet.fromPhrase(
  "YOUR_WALLET_PASSPHRASE"
  orandInstance.rpcProvider
);

const newEpoch = await orandInstance.newPrivateEpoch();
console.log(newEpoch);
console.log(await orandInstance.publish(newEpoch, wallet));
```

Publish a new epoch to blockchain.

**Params:**

- `proof`: A epoch of Orand, it can obtain by `getPrivateEpoch()`, `newPrivateEpoch()`
- `wallet`: The wallet that going to pay for transaction fee

**Result:**

This method will return `ethers.ContractTransactionResponse`.

**Output:**

```js
{
  provider: JsonRpcProvider {},
  blockNumber: null,
  blockHash: null,
  index: undefined,
  hash: '0x9c9330c9eedb694c5a7a0f16fd90484cf6b341fe3ec37064707970fb7bb0bbcb',
  type: 2,
  to: '0xfB40e49d74b6f00Aad3b055D16b36912051D27EF',
  from: '0x7Ba5A9fA3f3BcCeE36f60F62a6Ef728C3856b8Bb',
  nonce: 27,
  gasLimit: 108996n,
  gasPrice: undefined,
  maxPriorityFeePerGas: 1000000n,
  maxFeePerGas: 1000210n,
  data: '0x7fcde1c20000000000000000000000003fc4344b63fb1ab35a406cb90ca7310ec868758516b9fc8b15a9860441e78da6667a2923c6dfb0e3b59edba90e237c216ba2e9fea8750d1a022cfb3479115336d90d8ba1392573791bb947681b755b23c8bfd0b0cc87f56c81b532b3878bcb44658ae06e2466f684c4b57dce4809e76462e0a827099620a1593ddb85f9d679649f992f4f519d8b963e2972d2ec2c0d6adbd7e1c23b4cbd80caafecc3a6da03fc49e01301bcd701f032569139c705bd9454832d79000000000000000000000000985b293668b106496f6787e540f4d5ea5ace41e9737afd298e5ef5e957781e5964c0622f76f1dc94d3447ebe0db9e3a21bfce2757a51418f22fc42d6f66ed26382577392cd48c75e77e316d43285185b4b92d09f3f102de22d018ee768af6b26c120a918993a7d740e9d10e3db3f26d1bdada402da2f3acb4e7eb037d496bb1b2ebd94c628f88bddec2324fce1f3d82d675b35184a578b8fe55e2c1ce1b0fe7181bb5a84fa50333889ff9bcc9bb86377c22f3237',
  value: 0n,
  chainId: 28122024n,
  signature: Signature { r: "0xe41149214e6e78352f9b6cd371519edaecdc38684885d347458ddb2b0a7bc87d", s: "0x7be900ca6af85244aeeb887c765f2fae869803f6ce74afd4cac3b09d9307fc14", yParity: 0, networkV: null },
  accessList: []
}
```

### getPublicKey()

```ts

```

### static transformProof(proof: OrandEpoch): OrandProof

This is a helper function that allow you to tranfor proof for smart contract parameters.
