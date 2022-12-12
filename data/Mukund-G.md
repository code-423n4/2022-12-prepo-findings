## INDEX

| No. | issue                                                                                  |
|-----|----------------------------------------------------------------------------------------|
| 1   | <X> += <Y> COST MORE GAS THAN <X> = <X>+<Y> FOR STATE VARIABLE                         |
| 2   | USAGE OF UINT SMALLER THAN 32 BYTES( 256 BITS)  INCURS OVERHEAD                        |
| 3   | THE RESULT OF FUNCTION CALLS SHOULD BE CACHED RATHER THAN FETCHING INSIDE THE FUNCTION |
| 4   | FUNCTION GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED `PAYABLE`      |
| 5   | USING FUNCTION INSTEAD OF MODIFIERS CAN SAVE GAS                                       |


## 1. <X> += <Y> COST MORE GAS THAN <X> = <X>+<Y> FOR STATE VARIABLE
Using the addition operator instead of plus-equals saves gas
[DepositRecord.sol#L31](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L31)
[WithdrawHook.sol#L64](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L64)
[WithdrawHook.sol#L71](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L71)

## 2. USAGE OF UINT SMALLER THAN 32 BYTES( 256 BITS)  INCURS OVERHEAD
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
[DepositTradeHelper.sol#L12](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L12)

## 3. THE RESULT OF FUNCTION CALLS SHOULD BE CACHED RATHER THAN FETCHING INSIDE THE FUNCTION
[TokenSender.sol#L37](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L37)

## 4. FUNCTION GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED `PAYABLE`
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. 
[Collateral.sol#L80-L115](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L80-L115)
[DepositHook.sol#L54-L79](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L54-L79)
[DepositRecord.sol#L40-L53](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L40-L53)
[ManagerWithdrawHook.sol#L19-L33](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L19-L33)
[PrePOMarket.sol#L109-L130](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L109-L130)
[TokenSender.sol#L45-L62](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L45-L62)

## 5. USING FUNCTION INSTEAD OF MODIFIERS CAN SAVE GAS
use function instead of modifiers if possible because it will save gas
[DepositHook.sol#L27-L30](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L27-L30)
[DepositRecord.sol#L18-L21](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L18-L21)
