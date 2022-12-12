## [G-0x] x = x - y costs less gas than x -= y, same for addition
You can replace all -= and += occurrences to save gas. It occurs frequently in code. Here is an example:
[DepositRecord.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L31)