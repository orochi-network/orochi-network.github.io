## Unlocking the Web3 Universe: Orochi Network's Oracle Service

Imagine building a Web3 application that thrives on real-world data, free from centralized control. This is the vision behind Orochi Network's Oracle service, a powerful tool poised to revolutionize the way DApps interact with the external world.

Traditionally, DApps have struggled to access external data sources, relying on centralized oracles – single points of failure susceptible to manipulation and bias. Orochi's Oracle shatters these limitations, offering a decentralized, secure, and versatile solution for feeding accurate and verifiable data into your DApps.

## So, what exactly can the Orochi Oracle do?

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

## Deployed Platform

Oracle V1 was deployed on following smart contract platform.

### Testnet

| Network Name          | Address                                                                                                                             |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Ancient8 Testnet      | [0x37fe3DeADd810aebea4289E3fC2F2dEf4630d265](https://scanv2-testnet.ancient8.gg/address/0x37fe3DeADd810aebea4289E3fC2F2dEf4630d265) |
| Unicorn Ultra Nebulas | [0x22a47dFcE099e26C9FffBfa5e356cdfD0DC38844](https://testnet.u2uscan.xyz/address/0x22a47dFcE099e26C9FffBfa5e356cdfD0DC38844)        |

### Mainnet

N/A

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

**Errors:** The contract defines several errors to handle potential issues:

- `ExistedApplication`: Application ID already exists.
- `InvalidApplication`: Invalid application ID.
- `InvalidApplicationName`: Invalid application name.
- `InvalidRoundNumber`: Invalid round number.
- `UndefinedRound`: Round is not yet defined.
- `InvalidDataLength`: Invalid data length.
- `UnableToPublishData`: Failed to publish data.

**ApplicationMetadata Struct:** Stores application information:

- `name`: Bytes16 representing the application name.
- `lastUpdate`: uint64 timestamp of the last update.
- `round`: uint64 current round number.

**Functions:**

1. `getRound(uint32 appId) external view returns (uint256 round):`
   - Returns the current round number for a given application ID.
2. `getLastUpdate(uint32 appId) external view returns (uint256 lastUpdate):`
   - Returns the timestamp of the last update for a given application ID.
3. `getApplication(uint32 appId) external view returns (ApplicationMetadata memory app):`
   - Returns application metadata, including name, last update, and round.
4. `getData(uint32 appId, uint64 round, bytes20 identifier) external view returns (bytes32 data):`
   - Returns data for a specific application, round, and identifier.
5. `getDataLte(uint32 appId, uint64 targetRound, bytes20 identifier) external view returns (bytes32 data):`
   - Returns data for a given application and identifier, where round is less than or equal to the target round.
6. `getDataGte(uint32 appId, uint64 targetRound, bytes20 identifier) external view returns (bytes32 data):`
   - Returns data for a given application and identifier, where round is greater than or equal to the target round.
7. `getLatestData(uint32 appId, bytes20 identifier) external view returns (bytes32 data):`
   - Returns the latest data for a given application and identifier.

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
    return (_getPrice(srcToken) * 10 ** 18) / (_getPrice(dstToken));
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
