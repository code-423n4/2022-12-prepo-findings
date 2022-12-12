## [L-01] No constructor / uninitialized constructor for upgradeable contract
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L29
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L16

While the smart contracts in scope of this issue are initialized in proxy contract, leaving them uninitalized may lead to unexpected behaviours, as anyone may initialize them. Best practise in this case is addimg "initializer" midifier to the constructor to disallow that. Please reffer to the documentation: https://docs.openzeppelin.com/contracts/4.x/api/proxy#TransparentUpgradeableProxy

```
" ...An uninitialized contract can be taken over by an attacker. This applies to both a proxy and its implementation contract, which may impact the proxy... "
```

## [L-02] No storage gap set for upgradeable smart contract
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L28
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L15

Even though the smart contracts in scope are not being inherited by any smart contract, it's a good practise to create `uint256[50 - <maxStorageLot>] __gap;`, hwich prevents storage clashing in proxy, in case that the contract was to be inherited from in the future.

Please reffer to https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps for details.


## [L-04] If depositsAllowed is false && Collateral address is set, it will brick deposits
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L53
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L44
if depositsAllowed is false && Collateral address is set, it will brick deposits

## [L-05] __gap + storage slots should sum up to 50 in upgradeable contracts
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/SafeAccessControlEnumerableUpgradeable.sol#L65
Even though this contract is not in scope, some contracts that are in scope inherit from it and are directly impacted by this issue.
All storage slots used with `__gap` array elements count should sum up to constant number, usually 50 - not updating it whenever storage layout changes may shift whole storage layout and lead to unxepected behaviour. And there are 2 storage slots already used by mappings in this contract, which together with the gap sum up to **52** slots.
Actually, documentation quoted in the comment above the `__gap` says just that:

```
The size of the __gap array is calculated so that the amount of storage used by a contract always adds up to the same number (in this case 50 storage slots).
```

## [NC-01] Avoid using magic numbers
```
    uint256 _collateralMintAmount = (_amountAfterFee * 1e18) / baseTokenDenominator;
    uint256 _baseTokenAmount = (_amount * baseTokenDenominator) / 1e18;
```

## [NC-02] use most recent Solidity version
This issue concerns all the contracts in scope

## [NC-03] Unresolved TODOs
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L15
```
15:  WithdrawERC20, // TODO: Access control when WithdrawERC20 updated
```

## [NC-04] Collateral code doesn't match documentation
According to docs https://docs.prepo.io/concepts/prect : "preCT is backed by a basket of yield bearing USD stablecoins", while Collateral allows only for one token. Documentation should be probably updated to reflect that.
