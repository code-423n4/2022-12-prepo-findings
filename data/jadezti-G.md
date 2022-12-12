
|         | Issue                                                                        | Instances |
| ------- |:---------------------------------------------------------------------------- |:---------:|
| [GAS-1] | Using `unchecked` math operations when no overflow/underflow                    |     5     |


### [GAS-1] AUsing `unchecked` math operations when no overflow/underflow

*Instances: (5)*:


```solidity
File: apps/smart-contracts/core/contracts/Collateral.sol

//@audit - use unchecked operation, see Line 46
50:   uint256 _amountAfterFee = _amount - _fee;  

//@audit - use unchecked operation, see Line 65-66
70:   uint256 _baseTokenAmountAfterFee = _baseTokenAmount - _fee;  
```
[Link to code](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol)


```solidity
File: apps/smart-contracts/core/contracts/DepositRecord.sol

//@audit - use unchecked operation, see Line 29
31:    globalNetDepositAmount += _amount; 

//@audit - use unchecked operation, see Line 30
32:    userToDeposits[_sender] += _amount;

//@audit - use unchecked operation, the `if ()` statement guarantees no underflow
36:    if (globalNetDepositAmount > _amount) { globalNetDepositAmount -= _amount; }
```

