## Smart Contract Integration

Your smart contract that consumes Orand's randomness needs the interface of `IOrandConsumerV2` to be implemented, which was described below:

```solidity
// SPDX-License-Identifier: Apache-2.0
pragma solidity ^0.8.0;

error InvalidProvider();

interface IOrandConsumerV2 {
  // Consume the verifiable randomness from Orand provider
  // Return false if you want to stop batching
  function consumeRandomness(uint256 randomness) external returns (bool);
}
```

### Dice Game Example

The game is quite easy. You roll the dice and `Orand` will give you the verifiable randomness so you can calculate the dice number.

```solidity
// SPDX-License-Identifier: Apache-2.0
pragma solidity ^0.8.0;
import '@openzeppelin/contracts/access/Ownable.sol';
import '../orand-v2/interfaces/IOrandConsumerV2.sol';

error WrongGuessingValue(uint128 guessing);

// Application should be an implement of IOrandConsumerV2 interface
contract DiceGame is IOrandConsumerV2, Ownable {
  // Set new provider
  event SetProvider(address indexed oldProvider, address indexed newProvider);

  // Fulfill awaiting result
  event Fulfilled(uint256 indexed gameId, uint256 guessed, uint256 indexed result);

  // New guess from player
  event NewGuess(address indexed player, uint256 indexed gameId, uint128 indexed guessed);

  // Game structure
  struct Game {
    uint128 guessed;
    uint128 result;
  }

  // Provider address
  address private orandProvider;

  // Game result storage
  mapping(uint256 => Game) private gameResult;

  // Total game
  uint256 private totalGame;

  // Fulfiled randomness
  uint256 private fulfilled;

  // We batching the radomness in one epoch
  uint256 private maximumBatching;

  // Only allow Orand to submit result
  modifier onlyOrandProvider() {
    if (msg.sender != orandProvider) {
      revert InvalidProvider();
    }
    _;
  }

  // Constructor
  constructor(address provider) {
    _setProvider(provider);
  }

  //=======================[  Internal  ]====================

  // Set provider
  function _setProvider(address provider) internal {
    emit SetProvider(orandProvider, provider);
    orandProvider = provider;
  }

  // Set provider
  function _getProvider() internal view returns (address) {
    return orandProvider;
  }

  //=======================[  Owner  ]====================

  // Set provider
  function setProvider(address provider) external onlyOwner returns (bool) {
    _setProvider(provider);
    return true;
  }

  //=======================[  OrandProviderV2  ]====================

  // Consume the result of Orand V2 with batching feature
  function consumeRandomness(uint256 randomness) external override onlyOrandProvider returns (bool) {
    // We keep batching < maximumBatching
    if (fulfilled < totalGame) {
      Game memory currentGame = gameResult[fulfilled];
      currentGame.result = uint128((randomness % 6) + 1);
      gameResult[fulfilled] = currentGame;
      emit Fulfilled(fulfilled, currentGame.guessed, currentGame.result);
      fulfilled += 1;
      return true;
    }
    return false;
  }

  //=======================[  External  ]====================

  // Player can guessing any number in range of 1-6
  function guessingDiceNumber(uint128 guessing) external returns (bool) {
    // Player only able to guessing between 1-6 since it's dice number
    if (guessing < 1 || guessing > 6) {
      revert WrongGuessingValue(guessing);
    }
    gameResult[totalGame] = Game({ guessed: guessing, result: 0 });
    emit NewGuess(msg.sender, totalGame, guessing);
    totalGame += 1;
    return true;
  }

  //=======================[  External View  ]====================

  // Get result from smart contract
  function getResult(uint256 gameId) external view returns (Game memory result) {
    return gameResult[gameId];
  }

  function getStateOfGame() external view returns (uint256 fulfill, uint256 total) {
    return (fulfilled, totalGame);
  }
}
```

In this example, the smart contract `DiceGame` was deployed at [0x3fc4344b63fb1AB35a406Cb90ca7310EC8687585](https://scanv2-testnet.ancient8.gg/address/0x3fc4344b63fb1AB35a406Cb90ca7310EC8687585)

The method `consumeRandomness(uint256 randomness)` should be restricted to `OrandProviderV2`. Here is its address on Ancient8 testnet [0xfB40e49d74b6f00Aad3b055D16b36912051D27EF](https://scanv2-testnet.ancient8.gg/address/0xfB40e49d74b6f00Aad3b055D16b36912051D27EF)
