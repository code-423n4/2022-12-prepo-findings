## INDEX

| No. | issue                                                                                  |
|-----|----------------------------------------------------------------------------------------|
| 1   | Unsafe use of transfer()/transferFrom()                         |
| 2   | Missing checks for address 0                        |
| 3   | Missing check for 0 uint value |
| 4   | File is missing NatSpec      |
| 5   | USE NAMED IMPORTS INSTEAD OF PLAIN `IMPORT ‘FILE.SOL’                                       |
| 6   | INITIALIZE() FUNCTION CAN BE CALLED BY ANYBODY                                       |
|7    | Contracts are not using their OZ Upgradeable counterparts    |
|8    | USE NAMED IMPORTS INSTEAD OF PLAIN `IMPORT ‘FILE.SOL’ |


## 1. Unsafe use of transfer()/transferFrom()
Use openzeppelin safeERC20 library instead because some token may not revert in case of failure and may cause loss of funds so its better to use `safeTransfer()`/`safeTransferFrom()`
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L49
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L76
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L82
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L42

## 2. Missing checks for address 0
In [DepositHook.sol#L77](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L77) function manager can set treasury to address 0 which will eventually lead to loss of fees because fees is transferred to treasury
[Collateral.sol#L85-L88](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L85-L88)
[Collateral.sol#L102-L105](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L102-L105)
[Collateral.sol#L107-L110](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L107-L110)
[Collateral.sol#L112-L115](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L112-L115)
[](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L50-L53)
[](DepositRecord.sol#L50-L53)

## 3. Missing check for 0 uint value
for example In function [DepositRecord.sol#L40-L43](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L40-L43) manager can set globalNetDepositCap to 0 and user will not able to deposit funds because of this check [DepositRecord.sol#L29](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L29)
and also in [PrePOMarketFactory.sol#L22-L34](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22-L34) function createMarket can set expiry time to 0 which will make market non expiring
some instance of this issue:
[DepositRecord.sol#L45-L48](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L45-L48) 
[WithdrawHook.sol#L91-L114](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L91-L114)

## 4. File is missing NatSpec
all the contract are in scope except interface is missing NatSpec consider adding them

## 5. USE NAMED IMPORTS INSTEAD OF PLAIN `IMPORT ‘FILE.SOL’
for example use it like  ` import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";`
[Collateral.sol#L4-L7](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L4-L7)
[DepositHook.sol#L4-L10](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L4-L10)
[DepositRecord.sol#L4-L5](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L4-L5)
[DepositTradeHelper.sol#L4-L6](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L4-L6)
[WithdrawHook.sol#L4-L7](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L4-L7)
[PrePOMarketFactory.sol#L4-L10](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L4-L10)

## 6. INITIALIZE() FUNCTION CAN BE CALLED BY ANYBODY
`initialize()` function can be called anybody when the contract is not initialized.
[PrePOMarketFactory.sol#L16](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L16)
[Collateral.sol#L34-L38](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L34-L38)

## 7. Contracts are not using their OZ Upgradeable counterparts     
The non-upgradeable standard version of OpenZeppelin’s library are inherited / used by the contracts.
It would be safer to use the upgradeable versions of the library contracts to avoid unexpected behaviour.

[DepositHook.sol#L10](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L10)
[DepositTradeHelper.sol#L6](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L6)
[LongShortToken.sol#L4-L6](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol#L4-L6)

## 8. USE NAMED IMPORTS INSTEAD OF PLAIN `IMPORT ‘FILE.SOL’ 
for example use it like import `{Ownable} from "@openzeppelin/contracts/access/Ownable.sol";`
[DepositHook.sol#L4-L10](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L4-L10)
[Collateral.sol#L4-L7](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L4-L7)
[DepositRecord.sol#L4-L5](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contra
