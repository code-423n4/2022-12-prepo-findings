# 1. Remove Shorthand Addition/Subtraction Assignment

Instead of using the shorthand of addition/subtraction assignment operators (`+=`, `-=`)
it costs less to remove the shorthand (`x += y` same as `x = x+y`) saves ~22 gas

There is 1 instance of this:

[File: NFTScoreRequirement.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol)

[Line 60](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L60)

```
Line 60:    score += IERC721(collection).balanceOf(account) > 0 ? collectionScore : 0;
```

# 2. Cache storage values in memory to reduce `SLOAD`s

Code can be optimized by lessening the amount of `SLOAD`s.

`SLOAD`s are expensive (~100 gas) in contrast to `MLOAD`s/`MSTORE`s (~3 gas)

Storage value should get cached in `memory`

There are 2 instances of this:

[File: NFTScoreRequirement.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol)

[NFTScoreRequirement.sol#L13-L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L13-L15), [NFTScoreRequirement.sol#L60](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L60)

```solidity
Line 13:  function _satisfiesScoreRequirement(address account) internal view virtual returns (bool) {
Line 14:    return _requiredScore == 0 || getAccountScore(account) >= _requiredScore;
Line 15:  }

Line 60:  score += IERC721(collection).balanceOf(account) > 0 ? collectionScore : 0;
```

# 3. Using `Storage` Instead of `Memory` for Structs/Arrays Saves Gas

When fetching data from a `storage` location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from `storage`, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new `memory` variable, they incur an additional `MLOAD` rather than a cheap stack read. Instead of declaring the variable with the `memory` keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incurring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires `memory`, or if the array/struct is being read from another `memory` array/struct.

Affected line of code [DepositTradeHelper.sol#L32](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L32)

# 4. If you don't use the named return variables when a function returns, you are wasting deployment gas.

By using named returns for local variables, you can reduce the amount of gas you use.

Here is one occurrence of this:

[File: AccountListCaller.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol)

[Line 15-17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol#L15-L17)

```
Line 15:  function getAccountList() external view override returns (IAccountList) {
Line 16:    return _accountList;
Line 17:  }
```

# 5. Make Function payable

Functions marked as `payable` are slightly cheaper than non-`payable` ones because the Solidity compiler inserts a check into non-`payable` functions requiring `msg.value` to be zero.

However, keep in mind that this optimization opens the door for a whole set of security considerations involving Ether held in contracts. More information at the following link [Solidity Compiler Discussion](https://github.com/ethereum/solidity/issues/12539)

Affected line of code:

[File: DepositHook.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol)

[DepositHook.sol#L43](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L43)

```solidity
Line 43:  function hook(address _sender, uint256 _amountBeforeFee, uint256 _amountAfterFee) external override onlyCollateral {
```