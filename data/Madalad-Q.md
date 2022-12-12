# QA Report

## 1. Openzeppelin draft dependency

The `IERC20Permit.sol` contract heredit from an OpenZeppelin contract who is still a draft and is not considered ready for mainnet use. OpenZeppelin contracts may be considered draft contracts if they have not received adequate security auditing or are liable to change with future development.

Ensure the development team is aware of the risks of using a draft contract or consider waiting until the contract is finalised.

Otherwise, make sure that development team are aware of the risks of using a draft OpenZeppelin contract and accept the risk-benefit trade-off.

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L6

## 2. `Collateral.initialize()` can be frontrun

Ensure that `initialize()` is called in the same transaction as the contract is deployed to prevent frontrunning

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L34-L38

## 3. Missing zero address check

`Collateral.deposit()` does not check that the `_recipient` input is not equal to the zero address.

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L45
