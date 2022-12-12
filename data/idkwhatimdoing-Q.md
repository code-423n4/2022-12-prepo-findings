# Avoid leaving a contract uninitialized.

An uninitialized contract can be taken over by an attacker. This applies to both a proxy and its implementation
contract, which may impact the proxy. To prevent the implementation contract from being used, you should invoke
the {_disableInitializers} function in the constructor to automatically lock it when it is deployed:

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L34
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo/PPO.sol#L53
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L16
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol#L56


# Use safeTransferFrom instead of transferFrom

OpenZeppelin’s documentation discourages the use of transferFrom() and transfer(), use safeTransferFrom() and safeTransfer() whenever possible as it can prevent loss of funds

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L49
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L76
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L82
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L26
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L31
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/WithdrawERC721.sol#L20
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L21
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/mini-sales/MiniSales.sol#L43
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/mini-sales/MiniSales.sol#L48
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol#L83
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L69
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L104
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L76
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol#L1014
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol#L1023
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo/PPO.sol#L102


# Amount can be zero after fee deduction

During deposit in line 48 the amount is greater than 0 in order to proceed with the deposit. However, after subtracting the _fee the _amount can be 0.

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L50


# Deposit function is not compatible with deflationary token

The actual deposited amount might be lower than the specified depositAmount of the function parameter.
This would lead to wrong interest rate calculations on the principal.
Recommend transferring the tokens first and comparing pre-/after token balances to compute the actual deposited amount.

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L45-L61