### [Gas-01] <x> = <x> + <y> costs less than <x> += <y> 

*Instances (1)*:
```solidity
File:   apps/smart-contracts/core/contracts/DepositRecord.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L31
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L32
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L36

```
```solidity
File:  apps/smart-contracts/core/contracts/WithdrawHook.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L64
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L71
```
```solidity
File:   packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L60
```

### [Gas-02] Arithmatic Operation should be unchecked due to previous ```if``` condition
```
if (globalNetDepositAmount > _amount) { globalNetDepositAmount -= _amount; }
```
*Instances (1)*:
```solidity
File:   apps/smart-contracts/core/contracts/DepositRecord.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L36
```

### []