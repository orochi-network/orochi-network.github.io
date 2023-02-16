## Smart Contract Integration

You smart contract that consume Orand's randomness need to implement the interface of `IOrandConsumerV1` which was described below:

```solidity
// SPDX-License-Identifier: Apache-2.0
pragma solidity ^0.8.0;

error InvalidProvider();

interface IOrandConsumerV1 {
  function consumeRandomness(uint256 randomness) external returns (bool);
}
```

### Dice Game Example

The game is quite easy, you roll the dice and `Orand` will give you the verifiable randomness so you can calculate the dice number.

```solidity
// SPDX-License-Identifier: Apache-2.0
pragma solidity ^0.8.0;
import '../interfaces/IOrandConsumerV1.sol';

// Application should be an implement of IOrandConsumerV1 interface
contract ExampleDice is IOrandConsumerV1 {
  // Provider address
  address internal orandProviderV1;

  // Result of the computation process
  uint256 internal result;

  // Set new provider
  event SetProvider(address indexed oldProvider, address indexed newProvider);

  // Receive new result from Orand
  event ReceivedNewResult(uint256 indexed diceResult, uint256 indexed randomness);

  // Only allow Orand to submit result
  modifier onlyOrandProviderV1() {
    if (msg.sender != orandProviderV1) {
      revert InvalidProvider();
    }
    _;
  }

  // Constructor
  constructor(address provider) {
    orandProviderV1 = provider;
    emit SetProvider(address(0), provider);
  }

  // Consume the result of Orand V1
  function consumeRandomness(uint256 randomness) external override onlyOrandProviderV1 returns (bool) {
    // calculate dice dot
    result = (randomness % 6) + 1;
    emit ReceivedNewResult(result, randomness);
    return true;
  }

  // Get result from smart contract
  function getResult() external view returns (uint256) {
    return result;
  }
}
```

In this example the `DiceExample` was deployed at [0x66681298BBbdf30a0B3Ec98cAbf41AA7669dc201](https://testnet.bscscan.com/address/0x66681298BBbdf30a0B3Ec98cAbf41AA7669dc201#code)

The method `consumeRandomness(uint256 randomness)` should be restricted to `OrandProviderV1`. Here is its address on BNB Chain testnet [0xF3455Bb39e8C9228f8701ECb5D5A177A77096593](https://testnet.bscscan.com/address/0xF3455Bb39e8C9228f8701ECb5D5A177A77096593#code)
