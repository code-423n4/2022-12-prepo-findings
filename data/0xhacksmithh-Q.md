### [Low-01] Should use recent version of solidity

- Use a solidity version of at least 0.8.13 to get the ability to use `using for`
 with a list of free functions
- Use a solidity version of at least 0.8.12 to get `string.concat()`
 instead of `abi.encodePacked(<str>,<str>)`

*Instances (16)*:
```solidity
File:   apps/smart-contracts/core/contracts/Collateral.sol
File:   apps/smart-contracts/core/contracts/DepositHook.sol
File:   apps/smart-contracts/core/contracts/DepositRecord.sol
File:   apps/smart-contracts/core/contracts/DepositTradeHelper.sol
File:   apps/smart-contracts/core/contracts/LongShortToken.sol
File:   apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol
File:   apps/smart-contracts/core/contracts/MintHook.sol
File:   apps/smart-contracts/core/contracts/PrePOMarket.sol
File:   apps/smart-contracts/core/contracts/PrePOMarketFactory.sol
File:   apps/smart-contracts/core/contracts/RedeemHook.sol
File:   apps/smart-contracts/core/contracts/TokenSender.sol
File:   apps/smart-contracts/core/contracts/WithdrawHook.sol

File:   packages/prepo-shared-contracts/contracts/AccountListCaller.sol
File:   packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol
File:   packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol
File:   packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol
```

### [Low-02] Absence of Zero address check during setting value of critical state variables

*Instances (15)*:
```solidity
File:   apps/smart-contracts/core/contracts/Collateral.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L30
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L86
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L103
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L108
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L113
```
```solidity
File:   apps/smart-contracts/core/contracts/DepositHook.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L55
```
```solidity
File:   apps/smart-contracts/core/contracts/DepositTradeHelper.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L15-L17
```
```solidity
File:   apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L20
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L25
``` 
```solidity
File:   apps/smart-contracts/core/contracts/PrePOMarket.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L49
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L110
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L115
```
```solidity
File:   apps/smart-contracts/core/contracts/TokenSender.sol
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L32
```
```solidity
File:   packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol#L21
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol#L12
```


### [Low-03] During setting decimal for token, Decimal value should properly checked
```
constructor(IERC20 _newBaseToken, uint256 _newBaseTokenDecimals) {
    baseToken = _newBaseToken; // @audit
    baseTokenDenominator = 10**_newBaseTokenDecimals;   // @audit
  }
```
Should use IERC20Metadata to properly cross check decimal of token

*Instances (4)*:
```solidity
File:   apps/smart-contracts/core/contracts/Collateral.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L31
```
```solidity
File:   apps/smart-contracts/core/contracts/PrePOMarket.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L30
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L31
```
```solidity
File:   apps/smart-contracts/core/contracts/TokenSender.sol
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L25
```



### [Low-04] ```transferOwnerShip()``` should be a 2 step process or some critical address set (like very import role) should be 2 step process

*Instances (1)*:
```solidity
File:   apps/smart-contracts/core/contracts/PrePOMarket.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L47
```


### [NC-01] Instead of huge Number try to use scientific notations

By using scientific notation, it will increase readability
like instead of using ```100000``` try to use ```10e6```

*Instances (4)*:
```solidity
File:   apps/smart-contracts/core/contracts/Collateral.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L19
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L20
```
```solidity
File:   apps/smart-contracts/core/contracts/DepositTradeHelper.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L12
```
```solidity
File:   apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L12
``` 


### [NC-02] Library imported but never used in contract

*Instances (1)*:
```solidity
File:   apps/smart-contracts/core/contracts/LongShortToken.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol#L6
```

### [NC-03] Should be a named return value whenever possible
*Instances (1)*:
```solidity
File:   apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L41
``` 