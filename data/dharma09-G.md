### 1.Use of constant keccak variables results in extra hashing (and so gas).


## Proof of Concept
[Collateral.sol#L21-L27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L21-L27)
[DepositRecord.sol#L14-L16](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L14-L16)
[ManagerWithdrawHook.sol#L13-L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L13-L15)
[WithdrawHook.sol#L22-L30](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L22-L30)
[TokenSender.sol#L26-L29](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L26-L29)

This results in the keccak operation being performed whenever the variable is used, increasing gas costs relative to just storing the output hash. Changing to immutable will only perform hashing on contract deployment which will save gas.

## Tools Used

[ethereum/solidity#9232 (comment)](https://github.com/ethereum/solidity/issues/9232#issuecomment-646131646)

## Recommended Mitigation Steps

Change the variable to be immutable rather than constant