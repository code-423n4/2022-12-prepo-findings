
| | issue |
| ----------- | ----------- |
| 1 | [expressions for constant values such as a call to keccak256(), should use immutable rather than constant](#1-expressions-for-constant-values-such-as-a-call-to-keccak256-should-use-immutable-rather-than-constant) |
| 2 | [Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate](#2-multiple-addressid-mappings-can-be-combined-into-a-single-mapping-of-an-addressid-to-a-struct-where-appropriate) |
| 3 | [state variables should be cached in stack variables rather than re-reading them from storage](#3-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage) |
| 4 | [Stack variable used as a cheaper cache for a state variable is only used once](#4-stack-variable-used-as-a-cheaper-cache-for-a-state-variable-is-only-used-once) |
| 5 | [Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `require()` or `if` statement](#5-add-unchecked--for-subtractions-where-the-operands-cannot-underflow-because-of-a-previous-require-or-if-statement) |
| 6 | [`<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables](#6-x--y-costs-more-gas-than-x--x--y-for-state-variables) |
| 7 | [not using the named return variables when a function returns, wastes deployment gas](#7-not-using-the-named-return-variables-when-a-function-returns-wastes-deployment-gas) |
| 8 | [can make the variable outside the loop to save gas](#8-can-make-the-variable-outside-the-loop-to-save-gas) |
| 9 | [require()/revert() strings longer than 32 bytes cost extra gas](#9-requirerevert-strings-longer-than-32-bytes-cost-extra-gas) |
| 10 | [require() or revert() statements that check input arguments should be at the top of the function](#10-require-or-revert-statements-should-be-used-sorted-from-cheapest-to-most-expensive) |
| 11 | [use a more recent version of solidity](#11-use-a-more-recent-version-of-solidity) |
| 12 | [using `calldata` instead of `memory` for read-only arguments in external functions saves gas](#12-using-calldata-instead-of-memory-for-read-only-arguments-in-external-functions-saves-gas) |
| 13 | [internal functions only called once can be inlined to save gas](#13) |
| 14 | [public functions not called by the contract should be declared external instead](#14-public-functions-not-called-by-the-contract-should-be-declared-external-instead) |
| 15 | [should use arguments instead of state variable](#15-should-use-arguments-instead-of-state-variable) |
| 16 | [Use assembly to check for address(0)](#16-use-assembly-to-check-for-address0) |
| 17 | [before some functions we should check some variables for possible gas save](#17-before-some-functions-we-should-check-some-variables-for-possible-gas-save) |
| 18 | [instead of calculating a statevar with keccak256() every time the contract is made pre calculate them before and only give the result to a constant](#18-instead-of-calculating-a-statevar-with-keccak256-every-time-the-contract-is-made-pre-calculate-them-before-and-only-give-the-result-to-a-constant) |



## 1. expressions for constant values such as a call to keccak256(), should use immutable rather than constant

- [TokenSender.sol#L26](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L26)
- [TokenSender.sol#L27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L27)
- [TokenSender.sol#L28](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L28)
- [TokenSender.sol#L29](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L29)

- [WithdrawHook.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L22)
- [WithdrawHook.sol#L23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L23)
- [WithdrawHook.sol#L24](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L24)
- [WithdrawHook.sol#L25](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L25)
- [WithdrawHook.sol#L26](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L26)
- [WithdrawHook.sol#L27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L27)
- [WithdrawHook.sol#L28](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L28)
- [WithdrawHook.sol#L29](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L29)
- [WithdrawHook.sol#L30](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L30)

- [DepositRecord.sol#L14](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L14)
- [DepositRecord.sol#L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L15)
- [DepositRecord.sol#L16](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L16)

- [Collateral.sol#L21](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L21)
- [Collateral.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L22)
- [Collateral.sol#L23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L23)
- [Collateral.sol#L24](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L24)
- [Collateral.sol#L25](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L25)
- [Collateral.sol#L26](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L26)
- [Collateral.sol#L27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L27)

- [DepositHook.sol#L17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L17)
- [DepositHook.sol#L18](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L18)
- [DepositHook.sol#L19](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L19)
- [DepositHook.sol#L20](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L20)
- [DepositHook.sol#L21](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L21)
- [DepositHook.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L22)
- [DepositHook.sol#L23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L23)
- [DepositHook.sol#L24](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L24)
- [DepositHook.sol#L25](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L25)

- [ManagerWithdrawHook.sol#L13](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L13)
- [ManagerWithdrawHook.sol#L14](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L14)
- [ManagerWithdrawHook.sol#L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L15)


## 2. Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations. 

`validCollateral` and `deployedMarkets` used together in a function once and can be placed in a single slot:
- [PrePOMarketFactory.sol#L13-L14](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L13-L14)


## 3. state variables should be cached in stack variables rather than re-reading them from storage

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses. 

most of the times this if statement will be true and we will save 100 gas at a small possibility of 3 gas loss , so cache `_mintHook` before the if statement
- [PrePOMarket.sol#L68](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L68)

`finalLongPayout`
- [PrePOMarket.sol#L82](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L82)

`redemptionFee`
- [PrePOMarket.sol#L90](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L90)

`_redeemHook` cache it before the if statement because its gonna usually be true (the value changes further in function should take actions accordingly)
- [PrePOMarket.sol#L93](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L93)

`collateral`
- [WithdrawHook.sol#L76](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L76)

`depositFee` cache before #L46
- [Collateral.sol#L46](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L46)

`depositHook` cache before if statement
- [Collateral.sol#L51](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L51)

`withdrawFee` cache before #L66
- [Collateral.sol#L66](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L66)

`withdrawHook` cache before if statement
- [Collateral.sol#L71](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L71)

`managerWithdrawHook`
- [Collateral.sol#L81](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L81)

`collateral` 
- [DepositHook.sol#L49](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L49)

if `_requiredScore` is not equal to 0, `_requiredScore` will be read twice from storage consider caching it before the line 
- [NFTScoreRequirement.sol#L14](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L14)


## 4. Stack variable used as a cheaper cache for a state variable is only used once

If the variable is only accessed once, it’s cheaper to use the state variable directly that one time, and save the 3 gas the extra stack assignment would spend

`_shortPayout`
- [PrePOMarket.sol#L82](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L82)

- [PrePOMarketFactory.sol#L42-L45](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L42-L45)


## 5. Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `require()` or `if` statement

require(a <= b); x = b - a => require(a <= b); unchecked { x = b - a }
if(a <= b); x = b - a => if(a <= b); unchecked { x = b - a }
this will stop the check for overflow and underflow so it will save gas

this is checked in the if statement #L81
- [PrePOMarket.sol#L82](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L82)

this cant underflow because its withdraw and can not be higher than the original value
- [WithdrawHook.sol#L76](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L76)

- [Collateral.sol#L50](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L50)

- [Collateral.sol#L70](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L70)


## 6. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
Using the addition operator instead of plus-equals saves gas

- [WithdrawHook.sol#L64](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L64)
- [WithdrawHook.sol#L71](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L71)

- [DepositRecord.sol#L31](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L31)
- [DepositRecord.sol#L32](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L32)
- [DepositRecord.sol#L36](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L36)


## 7. not using the named return variables when a function returns, wastes deployment gas

do not use return at the end of the function

- [PrePOMarketFactory.sol#L41](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41)


## 8. can make the variable outside the loop to save gas

consider making the stack variables before the loop which gonna save gas

2 instances here `collection` and `collectionScore`
- [NFTScoreRequirement.sol#L59](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L59)


## 9. require()/revert() strings longer than 32 bytes cost extra gas

Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas

- [NFTScoreRequirement.sol#L23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L23)


## 10. require() or revert() statements should be used sorted from cheapest to most expensive

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (2100 gas*) in a function that may ultimately revert in the unhappy case.

swap the position of these 2 requires for possible gas save
- [PrePOMarket.sol#L77-L78](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L77-L78)

swap the condition of `if and else statements` which will result in checking the cheaper condition first, possibly save gas because we check a argument instead of a state var(100 gas save) 
- [Collateral.sol#L47-L48](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L47-L48)

same logic as the last one
- [Collateral.sol#L67-L68](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L67-L68)



## 11. use a more recent version of solidity

Use a solidity version of at least 0.8.10 to have `external` calls skip contract existence checks if the external call has a return value
Use a solidity version of at least 0.8.12 to get string.concat() to be used instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use `using for` with a list of free functions


## 12. using `calldata` instead of `memory` for read-only arguments in external functions saves gas

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. 

2 instances in this line
- [PrePOMarketFactory.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22)

`setCollectionScores`
- [NFTScoreRequirement.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L22)

`removeCollections`
- [NFTScoreRequirement.sol#L35](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L35)

`removeCollections` 
- [DepositHook.sol#L75](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L75)

`setCollectionScores`
- [DepositHook.sol#L73](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L73)




## 13. internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

`_createPairTokens`
- [PrePOMarketFactory.sol#L41](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41)


## 14. public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents’ functions and change the visibility from external to public and can save gas by doing so. 

`setAllowedMsgSenders`
- [AllowedMsgSenders.sol#L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol#L15)

`setAccountList`
- [AccountListCaller.sol#L10](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol#L10)

`setRequiredScore`
- [NFTScoreRequirement.sol#L17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L17)

`setCollectionScores`
- [NFTScoreRequirement.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L22)

`removeCollections`
- [NFTScoreRequirement.sol#L35](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L35)


## 15. should use arguments instead of state variable

This will save near 97 gas

- [DepositRecord.sol#L42](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L42)


## 16. Use assembly to check for address(0)

saves 6 gas per instance

- [PrePOMarket.sol#L68](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L68)
- [PrePOMarket.sol#L93](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L93)

- [Collateral.sol#L51](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L51)
- [Collateral.sol#L71](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L71)
- [Collateral.sol#L81](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L81)


## 17. before some functions we should check some variables for possible gas save

before transfer we should check for amount being 0 so the function doesnt run when its not gonna do anything

check `_amount`
- [PrePOMarket.sol#L69](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L69)

check `_collateralAfterFee`
- [PrePOMarket.sol#L104](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L104)

`baseTokenAmount`
- [DepositTradeHelper.sol#L26](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L26)

`_collateralAmountMinted`
- [DepositTradeHelper.sol#L31](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L31)

`_amount`
- [Collateral.sol#L82](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L82)


## 18. instead of calculating a statevar with keccak256() every time the contract is made pre calculate them before and only give the result to a constant

- [TokenSender.sol#L26](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L26)
- [TokenSender.sol#L27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L27)
- [TokenSender.sol#L28](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L28)
- [TokenSender.sol#L29](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L29)

- [WithdrawHook.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L22)
- [WithdrawHook.sol#L23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L23)
- [WithdrawHook.sol#L24](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L24)
- [WithdrawHook.sol#L25](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L25)
- [WithdrawHook.sol#L26](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L26)
- [WithdrawHook.sol#L27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L27)
- [WithdrawHook.sol#L28](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L28)
- [WithdrawHook.sol#L29](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L29)
- [WithdrawHook.sol#L30](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L30)

- [DepositRecord.sol#L14](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L14)
- [DepositRecord.sol#L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L15)
- [DepositRecord.sol#L16](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L16)

- [Collateral.sol#L21](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L21)
- [Collateral.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L22)
- [Collateral.sol#L23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L23)
- [Collateral.sol#L24](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L24)
- [Collateral.sol#L25](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L25)
- [Collateral.sol#L26](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L26)
- [Collateral.sol#L27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L27)

- [DepositHook.sol#L17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L17)
- [DepositHook.sol#L18](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L18)
- [DepositHook.sol#L19](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L19)
- [DepositHook.sol#L20](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L20)
- [DepositHook.sol#L21](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L21)
- [DepositHook.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L22)
- [DepositHook.sol#L23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L23)
- [DepositHook.sol#L24](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L24)
- [DepositHook.sol#L25](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L25)

- [ManagerWithdrawHook.sol#L13](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L13)
- [ManagerWithdrawHook.sol#L14](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L14)
- [ManagerWithdrawHook.sol#L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L15)
