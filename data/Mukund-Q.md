## Unsafe use of transfer()/transferFrom()
Use openzeppelin safeERC20 library instead because some token may not revert in case of failure and may cause loss of funds so its better to use `safeTransfer()`/`safeTransferFrom()`
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L49
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L76
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L82
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L42

## Missing checks for address 0
In [DepositHook.sol#L77](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L77) function manager can set treasury to address 0 which will eventually lead to loss of fees because fees is transferred to treasury
[Collateral.sol#L85-L88](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L85-L88)
[Collateral.sol#L102-L105](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L102-L105)
[Collateral.sol#L107-L110](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L107-L110)
[Collateral.sol#L112-L115](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L112-L115)
[](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L50-L53)
[](DepositRecord.sol#L50-L53)

## Missing check for 0 uint value
for example In function [DepositRecord.sol#L40-L43](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L40-L43) manager can set globalNetDepositCap to 0 and user will not able to deposit funds because of this check [DepositRecord.sol#L29](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L29)
and also in [PrePOMarketFactory.sol#L22-L34](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22-L34) function createMarket can set expiry time to 0 which will make market non expiring
some instance of this issue:
[DepositRecord.sol#L45-L48](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L45-L48) 
[WithdrawHook.sol#L91-L114](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L91-L114)

## File is missing NatSpec
all the contract are in scope except interface is missing NatSpec consider adding them
