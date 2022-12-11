## Impact
Attack on AccountListCaller is able to call account list address from internal IAccountList.

## Proof of Concept
## PoC screen dump
https://github.com/gbadebosmith/ouch/blob/main/Attack%20on%20AccountListCaller%20able%20to%20call%20account%20list%20address%20from%20internal%20IAccountList%202022-12-11%20171458.jpeg
## PoC solidity attack file
https://github.com/gbadebosmith/ouch/blob/main/AttackAccountListCaller.sol
## Victim solidity file
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol#:~:text=IAccountList%20internal%20_accountList%3B

## Tools Used
Remix IDE local