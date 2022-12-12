## 1. Zero address/value checks should be implemented in the `constructor()`

Zero address/value checks should be implemented in the `constructor()` to mitigate any human errors. Not doing so could lead to redeploying the whole contract.

For example:

`File: Collateral.sol` [Line 29-32](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L29-L32)

```solidity
  constructor(IERC20 _newBaseToken, uint256 _newBaseTokenDecimals) {
    baseToken = _newBaseToken;
    baseTokenDenominator = 10**_newBaseTokenDecimals;
  }
```

As you can see, no zero address/value checks are made. Moreover, [`baseToken`](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L10) and [`baseTokenDenominator`](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L11) are both `immutable`, meaning they cannot be reassigned.

## 2. Lack of indexed events fields

Indexed parameters are treated as log topics instead of data, making them more easily accessible to off-chain tools. Up to three event parameters can have the `indexed` keyword. Note that indexing event parameters cost more gas.

Here are some instances of this issue:

File: `ITokenSender.sol` [Line 23](https://github.com/prepo-io/prepo-monorepo/blob/279e99a99ea27deaec91157cc79a5d5fdabf3d6e/packages/prepo-shared-contracts/contracts/interfaces/ITokenSender.sol#L23)

File: `IAllowedMsgSenders.sol` [Line 19](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/IAllowedMsgSenders.sol#L19)

File: `INFTScoreRequirement.sol` [line 17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol#L17)

File: `IPrePOMarketFactory.sol` [Line 14](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol#L14)

## 3. Use of an OpenZeppelin draft contract

The following instance imports an OpenZeppelin draft contract that isn't ready for mainnet use. Due to the nature of drafts, they may change and are not guaranteed stability. Using a draft contract in production can cause the permit function to fail unexpectedly and impact the protocol and its users.

`File: Collateral.sol` [Line 5](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L5)

```solidity
import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-ERC20PermitUpgradeable.sol";
```

## 4. Unresolved TODOs

Consider resolving open TODOs before deployment.

Instances found:

File: `TokenSender.sol` [Line 15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L15)

File: `IDepositRecord.sol` [Line 4](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/IDepositRecord.sol#L4)

File: `ERC20Mintable.sol` [Line 7](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/packages/prepo-shared-contracts/contracts/ERC20Mintable.sol#L7)

## 5. Limit line length

To comply with the [Solidity Style Guide](https://docs.soliditylang.org/en/develop/style-guide.html#maximum-line-length), lines should be limited to **120** characters max.

The instances listed below are over 120 characters long:

`File: PrePOMarketFactory.sol` [Line 22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22), [Line 28](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L28), [Line 41](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41)
`File: DepositTradeHelper.sol` [Line 22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L22), [Line 24](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L24), [Line 32](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L32)
`File: ManagerWithdrawHook.sol` [Line 17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L17), [Line 41](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L41)
`File: MintHook.sol` [Line 16](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L16)
`File: PrePOMarket.sol` [Line 42](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L42)
`File: DepositHook.sol` [Line 73](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L73)

## 6. Typo/spelling mistakes

File: `ICollateral.sol` [Line 48](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L48), [Line 95](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L95), [Line 100](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L100)

withdraw -> withdrawal

```
48:   * @param fee The new factor for calculating withdraw fees
...
95:   * expanded functionality such as pausability and withdraw limits.
...
100:   * Does not allow withdraw amounts small enough to result in
```

File: `ICollateral.sol` [Line 166](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L166)

```
  /// @return The factor used to calculate fees for depositinng
```

depositinng -> depositing

File: `MintHook.sol` [Line 12](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/MintHook.sol#L12)

```
   * @dev Since, minting of PrePOMarket positions will only be done by
```

Consider removing the unnecessary comma: Since, -> Since

## 7. Events are missing old value

Consider emitting both the old and new values for events that change a state variable.

Here are some instances of this issue:

File: `ICollateral.sol` [Line 38](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L38), [Line 44](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L44), [Line 50](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L50), [Line 56](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L56), [Line 62](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L62), [Line 68](https://github.com/prepo-io/prepo-monorepo/blob/49a7ed94272db013245d9364e69be713a8aef0a2/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L68)

```
38:  event ManagerChange(address manager);
...
44:  event DepositFeeChange(uint256 fee);
..
50:  event WithdrawFeeChange(uint256 fee);
...
56:  event DepositHookChange(address hook);
...
62:  event WithdrawHookChange(address hook);
...
68:  event ManagerWithdrawHookChange(address hook);
```

File: `ITokenSenderCaller.sol` [Line 17](https://github.com/prepo-io/prepo-monorepo/blob/e30c26cef750778c1db594322fc4bbe235205d8f/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol#L17)

```
  event TreasuryChange(address treasury);
```
