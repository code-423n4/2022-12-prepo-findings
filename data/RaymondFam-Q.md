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
`Collateral.sol` imports an Openzeppelin's draft abstract contract that is not ready for mainnet use. A contract of this nature has not been time tested due to inadequate security auditing and is liable to modification and changes in future development. As such, using a draft EIP712 in a production contract could result in the permit function failing to work as expected and impact both the protocol and the users.

Here is the instance entailed:

[File: Collateral.sol#L5](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L5)

```
import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-ERC20PermitUpgradeable.sol";
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
