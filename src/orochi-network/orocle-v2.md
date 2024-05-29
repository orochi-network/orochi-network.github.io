## Unlocking the Web3 Universe: Orochi Network's Oracle Service

Imagine building a Web3 application that thrives on real-world data, free from centralized control. This is the vision behind Orochi Network's Oracle service, a powerful tool poised to revolutionize the way DApps interact with the external world.

Traditionally, DApps have struggled to access external data sources, relying on centralized oracles – single points of failure susceptible to manipulation and bias. Orochi's Oracle shatters these limitations, offering a decentralized, secure, and versatile solution for feeding accurate and verifiable data into your DApps.

## Deployed Platform

Oracle V2 was deployed on following smart contract platform.

### Mainnet

| Network Name    | Address                                                                                                                                   |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| BNB Chain       | [0xcE978D96f761a0AE9Dd252E666504482B74A4713](https://bscscan.com/address/0xce978d96f761a0ae9dd252e666504482b74a4713)                      |
| X Layer mainnet | [0x5cb715ddb32d0facd9c5b9e3a10f2ffceba93285](https://www.okx.com/web3/explorer/xlayer/address/0x5cb715ddb32d0facd9c5b9e3a10f2ffceba93285) |

### Testnet

| Network Name             | Address                                                                                                                                        |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Ancient8 Testnet         | [0xd26Ea014930305B498C5826cF0712F877CCAF93f](https://scanv2-testnet.ancient8.gg/address/0xd26Ea014930305B498C5826cF0712F877CCAF93f)            |
| Unicorn Ultra Nebulas    | [0x70523434ee6a9870410960E2615406f8F9850676](https://testnet.u2uscan.xyz/address/0x70523434ee6a9870410960E2615406f8F9850676)                   |
| Sei Devnet               | [0x2cC3DF2fC577237A3Fa00634c3d90910211bdAc7](https://seitrace.com/address/0x2cC3DF2fC577237A3Fa00634c3d90910211bdAc7)                          |
| Saakuru TestNet          | [0xA346Ed72C477F5007866D6B9edCb1563af8cC23d](https://explorer.testnet.oasys.games/address/0xA346Ed72C477F5007866D6B9edCb1563af8cC23d)          |
| Zircuit TestNet          | [0x9eF4E958A94375436bA4071599BDF47f81E1d96f](https://explorer.zircuit.com/address/0x9eF4E958A94375436bA4071599BDF47f81E1d96f)                  |
| ZKFair TestNet           | [0x4A9f4f5acF2E11eb4795f228805c6577876559e1](https://testnet-scan.zkfair.io/address/0x4A9f4f5acF2E11eb4795f228805c6577876559e1)                |
| X Layer TestNet          | [0x471434636bE5da937d401971267Dd56C1696C073](https://www.okx.com/web3/explorer/xlayer-test/address/0x471434636bE5da937d401971267Dd56C1696C073) |
| ZKLink Nova              | [0xb0b5fFeF72c6ea620689CF947C0458fAD5cF58da](https://sepolia.explorer.zklink.io/address/0xb0b5fFeF72c6ea620689CF947C0458fAD5cF58da)            |
| Bsc Testnet              | [0xbC792147B026F2B998A97d6BC9718569Df79ec65](https://testnet.bscscan.com/address/0xbC792147B026F2B998A97d6BC9718569Df79ec65)                   |
| Arbitrum Sepolia Testnet | [0x4ec4C6E856d0ce4D4bc57b50B9234DcD7c6c31BE](https://sepolia.arbiscan.io/address/0x4ec4C6E856d0ce4D4bc57b50B9234DcD7c6c31BE)                   |
| Moonbase Alpha TestNet   | [0xCe4defA26b1820026136D71F2e00e291C479D656](https://moonbase.moonscan.io/address/0xCe4defA26b1820026136D71F2e00e291C479D656)                  |

## So, what exactly can the Orochi Oracle (Orocle) do?

- **Gather Diverse Data:** Access a vast pool of information, from financial markets and weather updates to social media sentiment and IoT sensor readings. The possibilities are endless, empowering DApps with real-time, relevant data.
- **Decentralized & Trustworthy:** Eliminate the risk of manipulation with a distributed network of nodes verifying and securing data integrity. No single entity controls the flow of information, fostering trust and transparency.
- **Highly Scalable & Efficient:** Designed to handle high volumes of data requests efficiently, ensuring your DApp performs smoothly even with complex data integrations.
- **Chain Agnostic:** Integrate seamlessly with various blockchain platforms, offering flexibility and wider reach for your DApp.
  But why is this important?

## The potential applications are vast:

- **Decentralized finance (DeFi):** Integrate real-world market data for dynamic pricing and risk management in DeFi protocols.
- **Prediction markets:** Enable accurate and trustworthy predictions based on real-time events and data.
- **Supply chain management:** Track goods movement and environmental conditions transparently across complex supply chains.
- **Gaming & Entertainment:** Create immersive experiences that react to real-world events and user behavior.

Orochi's Oracle is more than just a data feed; it's a gateway to a truly decentralized and data-driven future for Web3. By unlocking the power of real-world data, it empowers developers to build DApps that are not only innovative but also robust, secure, and truly impactful.

Ready to explore the possibilities? Dive into the world of Orochi Network and unleash the full potential of your Web3 vision.

### Installation

Installing `@orochi-network/contracts` will help you interactive with Orochi Network Oracle and VRF (Orand) much more easier.

```bash
npm i --save-dev @orochi-network/contracts
```

## Oracle Aggregator Overview

This section describes all methods of Oracle V1 that facilitate interaction from external smart contracts. To familiarize yourself with Oracle V1, the following terminology definitions may be helpful.

**appId:** Application ID on Oracle, our oracle support multiple applications there are two of them listed here:

| Application ID | Description      |
| -------------- | ---------------- |
| 1              | Asset Price      |
| 2              | Cross Chain Data |

**round:** Each application will have different round number, every time a new dataset submitted the round number will be increased by 1

**identifier:** Data identifier, it's a `bytes20` used to index data on smart contract, for asset price application `identifier` is token's symbol

**Note:**

- _Only 255 latest rounds will be cached on smart contract, you can not get more than 255 rounds from the current round by using `getData()` method_
- _Round is uint64, so 2^64 is the limit of number of round_

### Summary:

- This contract defines an interface for interacting with an oracle aggregator.
- It provides functions to retrieve application metadata, data, and round information.
- It's designed for read-only access, as all functions are marked as view.

### Key Points:

The IOrocleAggregatorV1 interface defines several methods:

- `request(uint256 identifier, bytes calldata data)`: This function is used to create a new request. It takes an identifier and data as parameters and returns a boolean indicating whether the request was successful.
- `fulfill(uint256 identifier, bytes calldata data)`: This function is used to fulfill a request. It also takes an identifier and data as parameters and returns a boolean indicating whether the fulfillment was successful.
- `getMetadata(uint32 appId, bytes20 identifier)`: This function is used to get the metadata of a given application. It takes an application ID and an identifier as parameters and returns the round and the last update time.
- `getData(uint32 appId, uint64 round, bytes20 identifier)`: This function is used to get the data of an application for a specific round. It takes an application ID, a round number, and an identifier as parameters and returns the data.
- `getLatestData(uint32 appId, bytes20 identifier)`: This function is used to get the latest data of an application. It takes an application ID and an identifier as parameters and returns the data.
- `getLatestRound(uint32 appId, bytes20 identifier)`: This function is used to get the latest round of an application. It takes an application ID and an identifier as parameters and returns the round number, the last update time, and the data.

## Example

Here is an example of `AssetPrice` consumer:

```solidity
// SPDX-License-Identifier: Apache-2.0
pragma solidity ^0.8.0;

import '@openzeppelin/contracts/access/Ownable.sol';
import '@orochi-network/contracts/IOracleAggregatorV1.sol';

contract ConsumerAssetPrice is Ownable {
  IOracleAggregatorV1 private oracle;

  event SetOracle(address indexed oldOracle, address indexed newOracle);

  constructor(address oracleAddress) {
    _setOracle(oracleAddress);
  }

  function _setOracle(address newOracle) internal {
    emit SetOracle(address(oracle), newOracle);
    oracle = IOracleAggregatorV1(newOracle);
  }

  /**
   * Get price of an asset based USD
   * @dev Token price will use 18 decimal for all token
   * @param identifier Asset identifier (e.g. BTC, ETH, USDT)
   * @return price
   */
  function _getPrice(bytes20 identifier) internal view returns (uint256) {
    return uint256(oracle.getLatestData(1, identifier));
  }

  /**
   * Get price of a pair
   * @dev Token price will use 18 decimal for all token
   * (e.g. BTC/ETH => srcToken='BTC' dstToken='ETH')
   * @param srcToken Asset identifier of source
   * @param dstToken Asset identifier of destination
   * @return price
   */
  function _getPriceOfPair(bytes20 srcToken, bytes20 dstToken) internal view returns (uint256) {
    return (_getPrice(srcToken) * 10 ** 9) / (_getPrice(dstToken));
  }

  /**
   * Allow owner to set new Oracle address
   * @param newOracle new Oracle address
   * @return success
   */
  function setOracle(address newOracle) external onlyOwner returns (bool) {
    _setOracle(newOracle);
    return true;
  }
}
```

Here is an example of `BitcoinSeller` that used `ConsumerAssetPrice`:

```solidity
// SPDX-License-Identifier: Apache-2.0
pragma solidity ^0.8.0;

import './ConsumerAssetPrice.sol';

contract BitcoinSeller is ConsumerAssetPrice {
  constructor(address provider) ConsumerAssetPrice(provider) {}

  function estimate(uint256 amount) external view returns (uint256 total) {
    total = _getPrice('BTC') * amount;
  }

  function ethOverBtc() external view returns (uint256 price) {
    price = _getPriceOfPair('ETH', 'BTC');
  }
}
```
