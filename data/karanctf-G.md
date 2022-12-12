## `10eX` use less gas then `10**X`

#### POC
For example, `10e3` represents the number 1000 with 3 decimal places, whereas `10**3` represents the number 1000 raised to the power of 3, or 1,000,000. Because the `10e3` notation is a simple representation of the number of decimal places, it requires less gas to execute than the `10**3` notation, which requires a calculation to determine the result of raising 10 to the power of 3.
```solidity
apps/smart-contracts/core/contracts/TokenSender.sol:33:    _outputTokenDecimalsFactor = 10**outputToken.decimals();
apps/smart-contracts/core/contracts/Collateral.sol:31:    baseTokenDenominator = 10**_newBaseTokenDecimals;
```


## Multiple address mappings can be combined into a single mapping of an `address` to a `struct`, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.
```solidity
apps/smart-contracts/core/contracts/PrePOMarketFactory.sol:13:  mapping(address => bool) private validCollateral;
apps/smart-contracts/core/contracts/DepositRecord.sol:11:  mapping(address => uint256) private userToDeposits;
apps/smart-contracts/core/contracts/DepositRecord.sol:12:  mapping(address => bool) private allowedHooks;
apps/smart-contracts/core/contracts/WithdrawHook.sol:20:  mapping(address => uint256) private userToAmountWithdrawnThisPeriod;
```
