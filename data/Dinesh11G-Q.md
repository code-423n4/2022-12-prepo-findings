### 1st Bug
Unsafe ERC20 Operation(s)

#### Impact
Issue Information: 
ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard.

It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.

To circumvent ERC20's approve functions race-condition vulnerability use OpenZeppelin's SafeERC20 library's safe{Increase|Decrease}Allowance functions.

In case the vulnerability is of no danger for your implementation, provide enough documentation explaining the reasonings.

Example
🤦 Bad:

IERC20(token).transferFrom(msg.sender, address(this), amount);
🚀 Good (using OpenZeppelin's SafeERC20):

import {SafeERC20} from "openzeppelin/token/utils/SafeERC20.sol";

// ...

IERC20(token).safeTransferFrom(msg.sender, address(this), amount);
🚀 Good (using require):

bool success = IERC20(token).transferFrom(msg.sender, address(this), amount);
require(success, "ERC20 transfer failed");

#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::49 => baseToken.transferFrom(msg.sender, address(this), _amount);
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::76 => baseToken.transfer(msg.sender, _baseTokenAmountAfterFee);
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::82 => baseToken.transfer(manager, _amount);
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::49 => collateral.getBaseToken().transferFrom(address(collateral), _treasury, _fee);
prepo-monorepo/apps/smart-contracts/token/contracts/mini-sales/MiniSales.sol::43 => paymentToken.transferFrom(
prepo-monorepo/apps/smart-contracts/token/contracts/mini-sales/MiniSales.sol::48 => saleToken.transfer(_recipient, _saleTokenAmount);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::219 => underlying.transferFrom(msg.sender, address(this), _amount),
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::340 => mAsset.transferFrom(msg.sender, address(this), _underlying),
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::494 => underlying.approve(unwrapper, massetReturned);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::537 => underlying.transfer(msg.sender, underlying_),
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::722 => underlying.approve(address(connector_), deposit);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::519 => mAsset.approve(address(recipient), unallocated);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockAave.sol::66 => IERC20(reserve).transfer(to, amount);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockMasset.sol::66 => IERC20(_input).transferFrom(msg.sender, address(this), _inputQuantity);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockMasset.sol::83 => IERC20(_output).transfer(_recipient, outputQuantity);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockRewardToken.sol::37 => IERC20(rewardToken).transfer(msg.sender, rewardAmount);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/rewards/MockRootChainManager.sol::30 => IERC20(rootToken).transferFrom(msg.sender, address(this), amount);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsContract.sol::76 => underlying.transfer(msg.sender, underlying_),
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsContract.sol::119 => // underlying.approve(unwrapper, massetRedeemed);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsManager.sol::27 => IERC20(_mAsset).approve(save, interestCollected);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsManager.sol::44 => IERC20(_mAsset).approve(save, bal);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/connectors/MockConnector.sol::23 => IERC20(mUSD).transferFrom(save, address(this), _amount);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/connectors/MockConnector.sol::28 => IERC20(mUSD).transfer(save, _amount);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/connectors/MockConnector.sol::33 => IERC20(mUSD).transfer(save, deposited);
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::83 => paymentToken.transferFrom(
prepo-monorepo/apps/smart-contracts/token/contracts/vesting/Vesting.sol::95 => _vestedToken.transfer(msg.sender, _claimableAmount);
prepo-monorepo/packages/prepo-shared-contracts/contracts/WithdrawERC721.sol::20 => IERC721(_erc721Tokens[i]).transferFrom(address(this), _recipients[i], _ids[i]);
```
#### Tools used
Manual code review



### 2nd Bug
Unspecific Compiler Version Pragma

#### Impact
Issue Information: 
Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

Example
🤦 Bad:

pragma solidity ^0.8.0;
🚀 Good:

pragma solidity 0.8.4;

#### Findings:
```
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/deps/SignatureVerifier.sol::25 => pragma solidity ^0.8.0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/IBVault.sol::2 => pragma solidity ^0.8.0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/ILockedERC20.sol::3 => pragma solidity ^0.8.0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin/InstantProxyAdmin.sol::3 => pragma solidity ^0.8.0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::3 => pragma solidity ^0.8.0;
```
#### Tools used
Manual code review




### 3rd Bug
Do not use Deprecated Library Functions

#### Impact
Issue Information: 
The usage of deprecated library functions should be discouraged.

This issue is mostly related to OpenZeppelin libraries.

Example
🤦 Bad:

use SafeERC20 for IERC20;

// ...

IERC20(token).safeApprove(spender, value);
🚀 Good:

use SafeERC20 for IERC20;

// ...

IERC20(token).safeIncreaseAllowance(spender, value);

#### Findings:
```
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::171 => IERC20(_mAsset).safeApprove(address(_savingsContract), 0);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::172 => IERC20(_mAsset).safeApprove(address(_savingsContract), type(uint256).max);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/GovernedMinterRole.sol::24 => _setupRole(DEFAULT_ADMIN_ROLE, INexus(_nexus).governor());
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/MassetHelpers.sol::30 => IERC20(_asset).safeApprove(_spender, 0);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/MassetHelpers.sol::31 => IERC20(_asset).safeApprove(_spender, 2**256 - 1);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/stakedTokenWrapper.sol::20 => rewardsToken.safeApprove(_stakedToken, 2**256 - 1);
```
#### Tools used
Manual code review