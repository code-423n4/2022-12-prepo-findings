## Events associated with setter functions
Consider having events associated with setter functions emit both the new and old values instead of just the new value.

Here are some of the instances entailed:

[File: TokenSenderCaller.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol)

```
13:    emit TreasuryChange(treasury);

22:    emit TokenSenderChange(address(tokenSender));
```
[File: Collateral.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol)

```
87:    emit ManagerChange(_newManager);

93:    emit DepositFeeChange(_newDepositFee);

99:    emit WithdrawFeeChange(_newWithdrawFee);

104:    emit DepositHookChange(address(_newDepositHook));

109:   emit WithdrawHookChange(address(_newWithdrawHook));

114:    emit ManagerWithdrawHookChange(address(_newManagerWithdrawHook));
```
## Un-indexed parameters in events
Consider indexing parameters for events, serving as logs filter when looking for specifically wanted data. Up to three parameters in an event function can receive the attribute `indexed` which will cause the respective arguments to be treated as log topics instead of data.

Here are some of the instances entailed:

[File: IAccountListCaller.sol#L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/IAccountListCaller.sol#L15)

```
  event AccountListChange(IAccountList accountList);
```
[File: IAllowedMsgSenders.sol#L19](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/IAllowedMsgSenders.sol#L19)

```
  event AllowedMsgSendersChange(IAccountList allowedMsgSenders);
```
[File: INFTScoreRequirement.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol)

```
17:  event RequiredScoreChange(uint256 score);

24:  event CollectionScoresChange(IERC721[] collections, uint256[] scores);
```
## Sanity checks at the constructor
Adequate zero address and zero value checks should be implemented at the constructor to avoid accidental error(s) that could result in non-functional calls associated with it particularly when assigning immutable variables.

Here are some of the instances entailed:

[File: Collateral.sol#L30-L31](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L30-L31)

```
    baseToken = _newBaseToken;
    baseTokenDenominator = 10**_newBaseTokenDecimals;
```
## OpenZeppelin draft abstract contract
The protocol imports Openzeppelin's draft abstract contracts that are not ready for mainnet use. Contracts of this nature have not been time tested due to inadequate security auditing and are liable to modification and changes in future development. As such, the use of draft EIP712 in production contracts could result in associated permit functions failing to work as expected and impact both the protocol and the users.

Here are the instances entailed:

[File: Collateral.sol#L5](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L5)

```
import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-ERC20PermitUpgradeable.sol";
```
[File: DepositTradeHelper.sol#L6](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L6)

```
import "@openzeppelin/contracts/token/ERC20/extensions/draft-IERC20Permit.sol";
```
## Roles setup in Collateral.sol
`__SafeAccessControlEnumerable_init()` would only setup DEFAULT-ADMIN-ROLE. No default assignments were executed to the following roles in `initialize()` of `Collateral.sol` despite these could separately be done outside the contract:

MANAGER_WITHDRAW_ROLE
SET_MANAGER_ROLE
SET_DEPOSIT_FEE_ROLE
SET_WITHDRAW_FEE_ROLE
SET_DEPOSIT_HOOK_ROLE
SET_WITHDRAW_HOOK_ROLE
SET_MANAGER_WITHDRAW_HOOK_ROLE

Consider having them included in [`initialize()`](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L34-L38), serving also as an emergency measure by allowing the contract owner to assume these specific roles when need be.

## Lines too long
Lines in source code are typically limited to 80 characters, but it’s reasonable to stretch beyond this limit when need be as monitor screens theses days are comparatively larger. Considering the files will most likely reside in GitHub that will have a scroll bar automatically kick in when the length is over 164 characters, all code lines and comments should be split when/before hitting this length. Keep line width to max 120 characters for better readability where possible.

Here are some of the instances entailed:

[File: DepositHook.sol#L73](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L73)
[File: DepositTradeHelper.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L22)
[File: DepositTradeHelper.sol#L24](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L24)
[File: DepositTradeHelper.sol#L32](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L32)
[File: ManagerWithdrawHook.sol#L17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L17)
[File: ManagerWithdrawHook.sol#L41](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L41)
[File: MintHook.sol#L16](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L16)
[File: PrePOMarket.sol#L42](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L42)
[File: PrePOMarketFactory.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22)
[File: PrePOMarketFactory.sol#L28](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L28)
[File: PrePOMarketFactory.sol#L41](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41)

## Initialization of settable variables at the constructor
Consider initializing the key settable variables below at the constructor so that the core function, `hook()`, in `DepositHook.sol` could become functional upon contract deployment:

[File: DepositHook.sol#L13-L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L13-L15)

```
  ICollateral private collateral;
  IDepositRecord private depositRecord;
  bool public override depositsAllowed;
```
## Open TODOs
Open TODOs can point to architecture or programming issues that still need to be resolved. Consider resolving them before deploying.

Here is an instance entailed:

[File: TokenSender.sol#L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L15)

```
  WithdrawERC20, // TODO: Access control when WithdrawERC20 updated
```
## Modularity on import usages
For cleaner Solidity code in conjunction with the rule of modularity and modular programming, use named imports with curly braces instead of adopting the global import approach.

For instance, the import instances below could be refactored as follows:

[File: WithdrawHook.sol#L4-L7](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L4-L7)

```
import {IWithdrawHook} from "./interfaces/IWithdrawHook.sol";
import {IDepositRecord} from "./interfaces/IDepositRecord.sol";
import {TokenSenderCaller} from "prepo-shared-contracts/contracts/TokenSenderCaller.sol";
import {SafeAccessControlEnumerable} from "prepo-shared-contracts/contracts/SafeAccessControlEnumerable.sol";
```