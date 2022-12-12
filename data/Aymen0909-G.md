# Gas Optimizations

## Summary

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1      | Remove unecessary lines of code |  1  |
| 2      | Use `unchecked` blocks to save gas  |  10 |
| 3      | `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables  |  5 |

## Findings

### 1- Remove unecessary lines of code :

In the `redeem` function inside the PrePOMarket contract, the following lines of code are unecessary and should be removed to save gas :

File: apps/smart-contracts/core/contracts/PrePOMarket.sol [Line 95](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L95)
```
uint256 _collateralAllowanceBefore = collateral.allowance(address(this), address(_redeemHook));
```

File: apps/smart-contracts/core/contracts/PrePOMarket.sol [Line 97](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L97)
```
_actualFee = _collateralAllowanceBefore - collateral.allowance(address(this), address(_redeemHook));
```

The above lines are used to calculated the `_actualFee` but following the code logic it's clear the we always have `_actualFee == _expectedFee`, this is the case because the `_redeemHook.hook` function is passed the following parameters : 
```
amountBeforeFee = _collateralAmount
amountAfterFee = _collateralAmount - _expectedFee
```

And thus the fee reduced from the allowance balance would be :

```
fee = amountBeforeFee - amountAfterFee = _collateralAmount - (_collateralAmount - _expectedFee)
fee = _expectedFee
```

Meaning that the allowanceBefore will be decreased by _expectedFee which is exactly equal to _actualFee calculted with the formula : 

```
_actualFee = _collateralAllowanceBefore - collateral.allowance(address(this), address(_redeemHook));
```

The `_expectedFee` value should be used directly and those lines of code should be removed to save gas.

### 2- Use `unchecked` blocks to save gas :

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block.

There are 10 instances of this issue:

File: apps/smart-contracts/core/contracts/Collateral.sol [Line 50](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L50)
```
uint256 _amountAfterFee = _amount - _fee;
```

The above operation cannot underflow due to the line :

[uint256 _fee = (_amount * depositFee) / FEE_DENOMINATOR;](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L46) 

Because `depositFee` is always less than `FEE_DENOMINATOR` we always have the following `_amount > _fee` and thus the operation should be marked unchecked 


File: apps/smart-contracts/core/contracts/Collateral.sol [Line 70](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L70)
```
uint256 _baseTokenAmountAfterFee = _baseTokenAmount - _fee;
```

The above operation cannot underflow due to the line : 

[uint256 _fee = (_baseTokenAmount * withdrawFee) / FEE_DENOMINATOR;](hhttps://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L65) 

Because `withdrawFee` is always less than `FEE_DENOMINATOR` we always have the following `_baseTokenAmount > _fee` and thus the operation should be marked unchecked


File: apps/smart-contracts/core/contracts/DepositHook.sol[Line 47](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L47)
```
uint256 _fee = _amountBeforeFee - _amountAfterFee;
```

The above operation cannot underflow because only collateral contract can call the hook function with `deposit` function which ensure that we always have `_amountBeforeFee > _amountAfterFee` as we have `_amount=_amountBeforeFee > _amountAfterFee=_amountAfterFee`, and thus the operation should be marked unchecked


File: apps/smart-contracts/core/contracts/WithdrawHook.sol [Line 74](hhttps://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L74)
```
uint256 _fee = _amountBeforeFee - _amountAfterFee;
```

The above operation cannot underflow because only collateral contract can call the hook function with `withdraw` function which ensure that we always have `_amountBeforeFee > _amountAfterFee` as we have `_baseTokenAmount=_amountBeforeFee > _baseTokenAmountAfterFee=_amountAfterFee`, and thus the operation should be marked unchecked


File: apps/smart-contracts/core/contracts/DepositRecord.sol [Line 36](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L36)
```
globalNetDepositAmount -= _amount;
```

The above operation cannot underflow due to the check [if (globalNetDepositAmount > _amount)](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L36) and should be marked unchecked


File: apps/smart-contracts/core/contracts/DepositRecord.sol [Line 82](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L82)
```
uint256 _shortPayout = MAX_PAYOUT - finalLongPayout;
```

The above operation cannot underflow due to the check [if (finalLongPayout <= MAX_PAYOUT)](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L81) and should be marked unchecked


File: apps/smart-contracts/core/contracts/WithdrawHook.sol [Line 64](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L64)
```
globalAmountWithdrawnThisPeriod += _amountBeforeFee;
```

The above operation cannot overflow due to the check :

[require(globalAmountWithdrawnThisPeriod + _amountBeforeFee <= globalWithdrawLimitPerPeriod, "global withdraw limit exceeded");](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L63)

This check ensures that `globalAmountWithdrawnThisPeriod + _amountBeforeFee` is always less than `globalWithdrawLimitPerPeriod` and thus the operation can't overflow and should be marked unchecked


File: apps/smart-contracts/core/contracts/WithdrawHook.sol [Line 71](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L71)
```
userToAmountWithdrawnThisPeriod[_sender] += _amountBeforeFee;
```

The above operation cannot overflow due to the check :

[require(userToAmountWithdrawnThisPeriod[_sender] + _amountBeforeFee <= userWithdrawLimitPerPeriod, "user withdraw limit exceeded");](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L70)

This check ensures that `userToAmountWithdrawnThisPeriod[_sender] + _amountBeforeFee` is always less than `userWithdrawLimitPerPeriod` and thus the operation can't overflow and should be marked unchecked


File: apps/smart-contracts/core/contracts/DepositRecord.sol [Line 31](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L31)
```
globalNetDepositAmount += _amount;
```

The above operation cannot overflow due to the check :

[require(_amount + globalNetDepositAmount <= globalNetDepositCap, "Global deposit cap exceeded");](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L29)

This check ensures that `_amount + globalNetDepositAmount` is always less than `globalNetDepositCap` and thus the operation can't overflow and should be marked unchecked


File: apps/smart-contracts/core/contracts/DepositRecord.sol [Line 32](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L32)
```
userToDeposits[_sender] += _amount;
```

The above operation cannot overflow due to the check :

[require(_amount + userToDeposits[_sender] <= userDepositCap, "User deposit cap exceeded");](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L30)

This check ensures that `_amount + userToDeposits[_sender]` is always less than `userDepositCap` and thus the operation can't overflow and should be marked unchecked


### 3- `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables :

Using the addition operator instead of plus-equals saves **113 gas** as explained [here](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8)

There are 5 instances of this issue:

File: apps/smart-contracts/core/contracts/DepositRecord.sol [Line 31-32](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L31-L32)
```
globalNetDepositAmount += _amount;
userToDeposits[_sender] += _amount;
```

File: apps/smart-contracts/core/contracts/DepositRecord.sol [Line 36](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L36)
```
globalNetDepositAmount -= _amount;
```

File: apps/smart-contracts/core/contracts/WithdrawHook.sol [Line 64](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L64)
```
globalAmountWithdrawnThisPeriod += _amountBeforeFee;
```

File: apps/smart-contracts/core/contracts/WithdrawHook.sol [Line 71](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L71)
```
userToAmountWithdrawnThisPeriod[_sender] += _amountBeforeFee;
```