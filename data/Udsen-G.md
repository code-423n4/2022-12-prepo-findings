## 1. CAN SAVE GAS BY DECLARING CONSTANTS AS **PRIVATE** INSTEAD OF **PUBLIC**

There are three occurrences of this issue.

Can declare the constant as `private` to save **gas**

    File: apps/smart-contracts/core/contracts/TokenSender.sol
    uint256 public constant MULTIPLIER_DENOMINATOR = 10000;

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L25

    File: apps/smart-contracts/core/contracts/DepositTradeHelper.sol
    uint24 public constant override POOL_FEE_TIER = 10000;

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L12

    File: apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol
    uint256 public constant PERCENT_DENOMINATOR = 1000000;

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L12