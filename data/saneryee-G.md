# Gas Optimizations Report

|         | Issue                                                                                                              | Instances |
| ------- | :----------------------------------------------------------------------------------------------------------------- | :-------: |
| [G-001] | x += y or x -= y costs more gas than x = x + y or x = x - y for state variables                                    |     3     |
| [G-002] | Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate |     1     |
| [G-003] | internal functions only called once can be inlined to save gas                                                     |     1     |
| [G-004] | Replace modifier with function                                                                                     |     4     |
| [G-005] | Use named returns for local variables where it is possible.                                                        |     1     |

## [G-001] x += y or x -= y costs more gas than x = x + y or x = x - y for state variables

### Impact

Using the addition operator instead of plus-equals saves 113 gas. Usually does not work with struct and mappings.

### Findings

Total:3

[apps/smart-contracts/core/contracts/WithdrawHook.sol#L64](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo//apps/smart-contracts/core/contracts/WithdrawHook.sol#L64)

```solidity
64:    globalAmountWithdrawnThisPeriod += _amountBeforeFee;
```

[apps/smart-contracts/core/contracts/DepositRecord.sol#L36](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo//apps/smart-contracts/core/contracts/DepositRecord.sol#L36)

```solidity
36:    if (globalNetDepositAmount > _amount) { globalNetDepositAmount -= _amount; }
```

[apps/smart-contracts/core/contracts/DepositRecord.sol#L31](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo//apps/smart-contracts/core/contracts/DepositRecord.sol#L31)

```solidity
31:    globalNetDepositAmount += _amount;
```

## [G-002] Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

### Impact

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key's keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

### Findings

Total:1

[apps/smart-contracts/core/contracts/DepositRecord.sol#L11-12](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo//apps/smart-contracts/core/contracts/DepositRecord.sol#L11-12)

```solidity
11:    mapping(address => uint256) private userToDeposits;
12:      mapping(address => bool) private allowedHooks;
```

## [G-003] internal functions only called once can be inlined to save gas

### Impact

Not inlining costs 20 to 40 gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.

### Findings

Total:1

[apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo//apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41)

```solidity
41:    function _createPairTokens(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 _longTokenSalt, bytes32 _shortTokenSalt) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {
```

## [G-004] Replace modifier with function

### Impact

Modifiers make code more elegant, but cost more than normal functions.

### Findings

Total:4

[packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol#L10](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo//packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol#L10)

```solidity
10:    modifier onlyAllowedMsgSenders() {
```

[apps/smart-contracts/core/contracts/WithdrawHook.sol#L32](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo//apps/smart-contracts/core/contracts/WithdrawHook.sol#L32)

```solidity
32:    modifier onlyCollateral() {
```

[apps/smart-contracts/core/contracts/DepositRecord.sol#L18](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo//apps/smart-contracts/core/contracts/DepositRecord.sol#L18)

```solidity
18:    modifier onlyAllowedHooks() {
```

[apps/smart-contracts/core/contracts/DepositHook.sol#L27](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo//apps/smart-contracts/core/contracts/DepositHook.sol#L27)

```solidity
27:    modifier onlyCollateral() {
```

## [G-005] Use named returns for local variables where it is possible.

### Impact

It is not necessary to have both a named return and a return statement.

### Findings

Total:1

[apps/smart-contracts/core/contracts/Collateral.sol#L45-L61](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo//apps/smart-contracts/core/contracts/Collateral.sol#L45-L61)

```solidity
45   function deposit(address _recipient, uint256 _amount) external override nonReentrant returns (uint256) {
46     uint256 _fee = (_amount * depositFee) / FEE_DENOMINATOR;
...
...
57:    uint256 _collateralMintAmount = (_amountAfterFee * 1e18) / baseTokenDenominator;
...
...
60     return _collateralMintAmount;
61   }
```
