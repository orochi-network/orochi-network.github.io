### Installation

Installing `@orochi-network/contracts` will help you interactive with Orochi Network Oracle and VRF (Orand) much more easier.

```bash
npm i --save-dev @orochi-network/contracts
```

### Dice Game Example

The game is quite easy. You roll the dice and `Orand` will give you the verifiable randomness so you can calculate the dice number. You can check the source code of the (Dice Game)[https://github.com/orochi-network/smart-contracts/blob/main/contracts/examples/DiceGame.sol].

If you request via smart contract, you can skip `Orand Code Integration` and `API Reference`. Just need to check the example is enough, and request randomness from Oracle by using this code:

```ts
// Request randomness from Orand
IOracleAggregatorV1(oracle).request(0, "0x");
```

- `oracle` is the address of Orochi Network's oracle
- `0` is Orand application id
- `"0x"` is null data
