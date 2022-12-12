## Summary
### Low Risk
|      | Issue                                                                                                               |
|------|---------------------------------------------------------------------------------------------------------------------|
| L-01 | `Collateral` withdrawal process does not match the documentation                                                    |
| L-02 | Use of draft dependencies                                                                                           |
| L-03 | Rugging vector in `Collateral`                                                                                      |
| L-04 | Users will be permanently blocked from `depositing` into `Collateral` after a certain threshold                     |
| L-05 | `_floorValuation` and `_ceilingValuation` are not validated                                                         |
| L-06 | AccountList manager can prevent users from redeeming Long and Short tokens                                          |
| L-07 | `TokenSender.send` amount sent to the recipient can be arbitrarily modified by the owner of `FixedUintValue`        |
| L-08 | `TokenSender.send` amount sent to the recipient can be arbitrarily modified by the `SET_PRICE_MULTIPLIER_ROLE` role |
| L-09 | `TokenSender.send` amount sent to the recipient can be arbitrarily modified by the `SET_SCALED_PRICE_LOWER_BOUND_ROLE` role |
| L-10 | `SET_WITHDRAWALS_ALLOWED_ROLE` can block withdrawals in `Collateral` at any time.                                   |


## Low
### [L‑01] `Collateral` withdrawal process does not match the documentation

Users can deposit and withdraw from `Collateral`.
As per the [documentation](https://docs.prepo.io/developer/core-contracts/Collateral#initiatewithdrawal):
```
Creates a request to allow a withdrawal for amount Collateral in a later block.
```

Looking at the [sequence diagram](https://docs.prepo.io/developer/sequence-diagrams#withdraw), the withdrawal is a 2-step process, where a user should first call `initiateWithdrawal`, before calling `withdraw` in a future block.

The issue is that this is not reflected in the code:
- there is no `initiateWithdrawal` function in `Collateral`
- users can call `withdraw` and [withdraw](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L77) their `baseToken`, without having to wait for `delayedWithdrawalExpiry`.

### Recommendation

Amend the documentation to match the current contract behavior, or make the necessary changes in `Collateral` to match the docs.



### [L‑02] Use of draft dependencies


`Collateral` uses the `ERC20PermitUpgradeable.sol` from `OpenZeppelin`, which is a [draft contract](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/master/contracts/token/ERC20/extensions/draft-ERC20PermitUpgradeable.sol).
Draft contracts may change with future developments and are not considered ready for use.

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L5
```solidity
5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-ERC20PermitUpgradeable.sol"; //@audit - L: use of draft

```


### [L‑03] Rugging vector in `Collateral`

`Collateral.withdraw` allows the `manager` to withdraw an arbitrary `_amount` of `baseToken` from `Collateral`.

The only check is in the `ManagerWithdrawHook.hook` call, where it checks the withdrawal does not drop the amount of `_baseToken` below `(depositRecord.getGlobalNetDepositAmount() * minReservePercentage) / PERCENT_DENOMINATOR`

A `manager` and the account holding `SET_MIN_RESERVE_PERCENTAGE_ROLE` can perform the following steps:

1 - call `ManagerWithdrawHook.setMinReservePercentage(PERCENT_DENOMINATOR)`
2 - call `Collateral.managerWithdraw(baseToken.balanceOf(address(this)))`

And rug all the `baseToken` in `Collateral`, stealing from depositors who are then unable to `withdraw` the `baseToken` they are entitled to.

The reason this attack is currently possible is because `Collateral.deposit` does not directly send the tokens to `StrategyController`. After a user deposit, the tokens are sitting in `Collateral` until the tokens are transferred to `StrategyController`, making this attack possible.

### Recommendation


Looking at the [deposit flow diagram](https://docs.prepo.io/developer/sequence-diagrams#deposit), a possible mitigation is to make the deposit process atomic, ensuring tokens are deposited into the `Strategy` in the same transaction as the users deposit into `Collateral`, to make the attack impossible (or at least lower its impact).

### [L‑04] Users will be permanently blocked from `depositing` into `Collateral` after a certain threshold

Upon withdrawing from `Collateral`, `DepositRecord.recordWithdrawal` is called to update the deposit states.
But it does not lower the user deposit states, only the global one.

The issue is that [this check](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/DepositRecord.sol#L30) prevents users from depositing if `userToDeposits[user]` exceeds `userDepositCap`

Because `userToDeposits[user]` is not updated during a withdrawal, this means that there will be a point where a user will not be able to deposit anymore - once their total historical deposit reaches `userDepositCap`.

This does impact the protocol negatively, as it poses an inconvenience on users. Users can simply use another EOA, but for smart contract wallets/multi-sigs, this means having to deal with the burden of re-deploying a new wallet and the logistics associated with it.

### Recommendation

Subtract the `_amount` from `userToDeposits[_sender]` in `DepositRecord.recordWithdrawal`.

You will also need to change the definition of `DepositRecord.recordWithdrawal` to include a `_sender` parameter.

This will also make the code match the [documentation](https://docs.prepo.io/developer/core-contracts/CollateralDepositRecord#recordwithdrawal), which specifies:

```
finalAmount is subtracted from both the global and account-specific deposit totals.
```

Note that `IDepositRecord` describes on the other hand that `recordWithdrawal` does not deduct from `msg.sender`'s deposit total.

It is not clear which is the intended behavior, but for the reasons enounced above, `recordWithdrawal` should update the account-specific deposit totals.

### [L‑05] `_floorValuation` and `_ceilingValuation` are not validated

The `PrePOMarket` constructor validates most parameters, but does not do so for `_floorValuation` and `_ceilingValuation`.

More specifically, it does not check `_ceilingValuation > _floorValuation`.


### Recommendation

Ensure all parameters are validated:

```diff
44:     require(_expiryTime > block.timestamp, "Invalid expiry");
45:     require(_ceilingLongPayout <= MAX_PAYOUT, "Ceiling cannot exceed 1");
+       require(_ceilingValuation > _floorValuation, "wrong  valuations");
```

### [L‑06] AccountList manager can prevent users from redeeming Long and Short tokens

`PrePOMarket.redeem` allows users to redeem `longAmount` Long and `shortAmount` Short tokens for collateral.

The function makes a call to `redeemHook.hook`, which [checks](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/RedeemHook.sol#L18) the user is in the AccountList.

A malicious AccountList manager can front run a user's call to `PrePOMarket.redeem` by calling `AccountList.set` to remove the user from this list, making the call to redeem revert.

The user cannot retrieve their collateral.


### Recommendation

Remove the `_accountList.isIncluded(sender)` check from `RedeemHook.hook`.
Note that there is no such check in `Collateral.withdraw`: the redeeming process in `PrePOMarket` should also get rid of it.

### [L‑07] `TokenSender.send` amount sent to the recipient can be arbitrarily modified by the owner of `FixedUintValue`


`TokenSender.send` is called upon deposit and withdrawal in `Collateral`, to send a user their reimbursement of fee in `PPO`.
The issue is that amount is computed based on the value returned in `_price.get()`.

This value can [be arbitrarily modified](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/FixedUintValue.sol#L10) in `FixedUintValue.set()` by the owner.

If set high enough:

```solidity
37: uint256 scaledPrice = (_price.get() * _priceMultiplier) / MULTIPLIER_DENOMINATOR;
38:     if (scaledPrice <= _scaledPriceLowerBound) return;
39:     uint256 outputAmount = (unconvertedAmount * _outputTokenDecimalsFactor) / scaledPrice;
```

`scaledPrice` will be high enough, leading to `outputAmount` being truncated and equal `0`.
The depositor does not receive any `PPO`.


### Recommendation

Consider using an oracle instead of an arbitrary value for the `PPO` price in `TokenSender.send`


### [L‑08] `TokenSender.send` amount sent to the recipient can be arbitrarily modified by the `SET_PRICE_MULTIPLIER_ROLE` role

This is similar to the previous issue, but it concerns another variable.

`TokenSender.send` is called upon deposit and withdrawal in `Collateral`, to send a user their reimbursement of fee in `PPO`.
The issue is that amount is computed based on the value returned in `_priceMultiplier`, which can [be arbitrarily modified](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/TokenSender.sol#L510) in by the `SET_PRICE_MULTIPLIER_ROLE` role.

If set high enough:

```solidity
37: uint256 scaledPrice = (_price.get() * _priceMultiplier) / MULTIPLIER_DENOMINATOR;
38:     if (scaledPrice <= _scaledPriceLowerBound) return;
39:     uint256 outputAmount = (unconvertedAmount * _outputTokenDecimalsFactor) / scaledPrice;
```

`scaledPrice` will be high enough, leading to `outputAmount` being truncated and equal `0`.
The depositor does not receive any `PPO`.


### Recommendation

The problem is that there is a mismatch between `_priceMuliplier`, that can be modified, and `MULTIPLIER_DENOMINATOR`, which is a constant.

`_priceMultiplier` should be immutable.


### [L‑09] `TokenSender.send` amount sent to the recipient can be arbitrarily modified by the `SET_SCALED_PRICE_LOWER_BOUND_ROLE` role

This is similar to the previous two issues, but it concerns another variable.

`TokenSender.send` is called upon deposit and withdrawal in `Collateral`, to send a user their reimbursement of fee in `PPO`.
The issue is that if the computed `scalePrice` is [lower](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/TokenSender.sol#L38) than `_scaledPriceLowerBound`, the user does not receive any `PPO`.

As this variable can be set to any value in [setScaledPriceLowerBound](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/TokenSender.sol#L56) by `SET_SCALED_PRICE_LOWER_BOUND_ROLE`, the latter can at any moment prevent users from receiving their reimbursement of fees in `PPO`.


### Recommendation

`setScaledPriceLowerBound` should have appropriate boundaries.

### [L‑10] `SET_WITHDRAWALS_ALLOWED_ROLE` can block withdrawals in `Collateral` at any time.

Users can deposit and withdraw from `Collateral`.
As per the [documentation](https://docs.prepo.io/developer/core-contracts/Collateral#initiatewithdrawal), users can 
create a request to allow a withdrawal in a later block

The issue is that [this additional check](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/WithdrawHook.sol#L58) makes it possible for the `SET_WITHDRAWALS_ALLOWED_ROLE` role to DOS withdrawals at any time, by calling [setWithdrawalsAllowed(false)](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/WithdrawHook.sol#L92).

### Recommendation

Either remove this function, or if deemed necessary for strategy reasons, consider adding a timelock to it.