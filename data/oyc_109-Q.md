

## [L-01] Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::44 => require(_expiryTime > block.timestamp, "Invalid expiry");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::59 => if (lastGlobalPeriodReset + globalPeriodLength < block.timestamp) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::60 => lastGlobalPeriodReset = block.timestamp;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::66 => if (lastUserPeriodReset + userPeriodLength < block.timestamp) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::67 => lastUserPeriodReset = block.timestamp;
```

## [L-02] Open TODOs

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

```
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::15 => WithdrawERC20, // TODO: Access control when WithdrawERC20 updated
```

## [L-03] decimals() not part of ERC20 standard

decimals() is not part of the official ERC20 standard and might fail for tokens that do not implement it. While in practice it is very unlikely, as usually most of the tokens implement it, this should still be considered as a potential issue.

```
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::33 => _outputTokenDecimalsFactor = 10**outputToken.decimals();
```

## [L-04] abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). Unless there is a compelling reason, abi.encode should be preferred. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

```
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::26 => bytes32 _salt = keccak256(abi.encodePacked(_longToken, _shortToken));
```

## [L-05] approve should be replaced with safeApprove or safeIncreaseAllowance() / safeDecreaseAllowance()

approve is subject to a known front-running attack. Consider using safeApprove() or safeIncreaseAllowance() or safeDecreaseAllowance() instead

```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::52 => baseToken.approve(address(depositHook), _fee);
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::54 => baseToken.approve(address(depositHook), 0);
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::72 => baseToken.approve(address(withdrawHook), _fee);
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::74 => baseToken.approve(address(withdrawHook), 0);
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::18 => collateral.getBaseToken().approve(address(collateral), type(uint256).max);
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::19 => collateral.approve(address(swapRouter), type(uint256).max);
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::94 => collateral.approve(address(_redeemHook), _expectedFee);
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::98 => collateral.approve(address(_redeemHook), 0);
```

## [L-06] Unsafe use of transfer()/transferFrom() with IERC20

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::49 => baseToken.transferFrom(msg.sender, address(this), _amount);
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::76 => baseToken.transfer(msg.sender, _baseTokenAmountAfterFee);
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::82 => baseToken.transfer(manager, _amount);
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::49 => collateral.getBaseToken().transferFrom(address(collateral), _treasury, _fee);
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::26 => _baseToken.transferFrom(msg.sender, address(this), baseTokenAmount);
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::31 => _collateral.transferFrom(msg.sender, address(this), _collateralAmountMinted);
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::69 => collateral.transferFrom(msg.sender, address(this), _amount);
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::104 => collateral.transfer(msg.sender, _collateralAfterFee);
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::21 => IPrePOMarket(msg.sender).getCollateral().transferFrom(msg.sender, _treasury, fee);
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::42 => _outputToken.transfer(recipient, outputAmount);
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::76 => collateral.getBaseToken().transferFrom(address(collateral), _treasury, _fee);
```

## [L-07] Events not emitted for important state changes

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

```
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::13 => function set(address[] calldata _accounts, bool[] calldata _included) external override onlyOwner {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::69 => function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::71 => function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) { super.setRequiredScore(_newRequiredScore); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::73 => function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::77 => function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) { super.setTreasury(_treasury); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::79 => function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); }
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::18 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::20 => function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::26 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::28 => function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::30 => function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::32 => function setTokenSender(ITokenSender tokenSender) public override onlyOwner { super.setTokenSender(tokenSender); }
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::60 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public override onlyRole(SET_ALLOWED_MSG_SENDERS_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::116 => function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::120 => function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) {
```

## [L-08] _safeMint() should be used rather than _mint() wherever possible

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::58 => _mint(_recipient, _collateralMintAmount);
prepo-monorepo/apps/smart-contracts/core/contracts/LongShortToken.sol::11 => function mint(address _recipient, uint256 _amount) external onlyOwner { _mint(_recipient, _amount); }
```

## [L-09] Upgradeable contract is missing a __gap[50] storage variable to allow for new storage variables in later versions

__gap is empty reserved space in storage that is recommended to be put in place in upgradeable contracts. It allows new state variables to be added in the future without compromising the storage compatibility with existing deployments

```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::5 => import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-ERC20PermitUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::8 => import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
```

## [L-15] Implementation contract may not be initialized

Implementation contract does not have a constructor with the initializer modifier therefore may be uninitialized. Implementation contracts should be initialized to avoid potential griefs or exploits.

```
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::8 => import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
```

## [N-1] Use a more recent version of solidity

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

```
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/LongShortToken.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::2 => pragma solidity =0.8.7;
```

## [N-2] Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 1000000), for better code readability

```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::19 => uint256 public constant FEE_DENOMINATOR = 1000000;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::20 => uint256 public constant FEE_LIMIT = 100000;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::12 => uint24 public constant override POOL_FEE_TIER = 10000;
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::12 => uint256 public constant PERCENT_DENOMINATOR = 1000000;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::30 => uint256 private constant FEE_DENOMINATOR = 1000000;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::31 => uint256 private constant FEE_LIMIT = 100000;
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::25 => uint256 public constant MULTIPLIER_DENOMINATOR = 10000;
```

## [N-3] Missing NatSpec

Code should include NatSpec

```
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::1 => // SPDX-License-Identifier: AGPL-3.0
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::1 => // SPDX-License-Identifier: AGPL-3.0
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::1 => // SPDX-License-Identifier: AGPL-3.0
prepo-monorepo/apps/smart-contracts/core/contracts/LongShortToken.sol::1 => // SPDX-License-Identifier: AGPL-3.0
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::1 => // SPDX-License-Identifier: AGPL-3.0
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::1 => // SPDX-License-Identifier: AGPL-3.0
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::1 => // SPDX-License-Identifier: AGPL-3.0
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::1 => // SPDX-License-Identifier: AGPL-3.0
```

## [N-4] Missing event for critical parameter change

Emitting events after sensitive changes take place, to facilitate tracking and notify off-chain clients following changes to the contract.

```
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::13 => function set(address[] calldata _accounts, bool[] calldata _included) external override onlyOwner {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::69 => function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::71 => function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) { super.setRequiredScore(_newRequiredScore); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::73 => function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::77 => function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) { super.setTreasury(_treasury); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::79 => function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); }
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::18 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::20 => function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::26 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::28 => function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::30 => function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::32 => function setTokenSender(ITokenSender tokenSender) public override onlyOwner { super.setTokenSender(tokenSender); }
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::60 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public override onlyRole(SET_ALLOWED_MSG_SENDERS_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::116 => function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::120 => function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) {
```

## [N-5] Expressions for constant values such as a call to keccak256(), should use immutable rather than constant

instances:

```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::21 => bytes32 public constant MANAGER_WITHDRAW_ROLE = keccak256("Collateral_managerWithdraw(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::22 => bytes32 public constant SET_MANAGER_ROLE = keccak256("Collateral_setManager(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::23 => bytes32 public constant SET_DEPOSIT_FEE_ROLE = keccak256("Collateral_setDepositFee(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::24 => bytes32 public constant SET_WITHDRAW_FEE_ROLE = keccak256("Collateral_setWithdrawFee(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::25 => bytes32 public constant SET_DEPOSIT_HOOK_ROLE = keccak256("Collateral_setDepositHook(ICollateralHook)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::26 => bytes32 public constant SET_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setWithdrawHook(ICollateralHook)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::27 => bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::17 => bytes32 public constant SET_COLLATERAL_ROLE = keccak256("DepositHook_setCollateral(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::18 => bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("DepositHook_setDepositRecord(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::19 => bytes32 public constant SET_DEPOSITS_ALLOWED_ROLE = keccak256("DepositHook_setDepositsAllowed(bool)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::20 => bytes32 public constant SET_ACCOUNT_LIST_ROLE = keccak256("DepositHook_setAccountList(IAccountList)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::21 => bytes32 public constant SET_REQUIRED_SCORE_ROLE = keccak256("DepositHook_setRequiredScore(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::22 => bytes32 public constant SET_COLLECTION_SCORES_ROLE = keccak256("DepositHook_setCollectionScores(IERC721[],uint256[])");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::23 => bytes32 public constant REMOVE_COLLECTIONS_ROLE = keccak256("DepositHook_removeCollections(IERC721[])");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::24 => bytes32 public constant SET_TREASURY_ROLE = keccak256("DepositHook_setTreasury(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::25 => bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("DepositHook_setTokenSender(ITokenSender)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::14 => bytes32 public constant SET_GLOBAL_NET_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setGlobalNetDepositCap(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::15 => bytes32 public constant SET_USER_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setUserDepositCap(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::16 => bytes32 public constant SET_ALLOWED_HOOK_ROLE = keccak256("DepositRecord_setAllowedHook(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::13 => bytes32 public constant SET_COLLATERAL_ROLE = keccak256("ManagerWithdrawHook_setCollateral(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::14 => bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("ManagerWithdrawHook_setDepositRecord(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::15 => bytes32 public constant SET_MIN_RESERVE_PERCENTAGE_ROLE = keccak256("ManagerWithdrawHook_setMinReservePercentage(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::26 => bytes32 public constant SET_PRICE_ROLE = keccak256("TokenSender_setPrice(IUintValue)");
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::27 => bytes32 public constant SET_PRICE_MULTIPLIER_ROLE = keccak256("TokenSender_setPriceMultiplier(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::28 => bytes32 public constant SET_SCALED_PRICE_LOWER_BOUND_ROLE = keccak256("TokenSender_setScaledPriceLowerBound(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::29 => bytes32 public constant SET_ALLOWED_MSG_SENDERS_ROLE = keccak256("TokenSender_setAllowedMsgSenders(IAccountList)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::22 => bytes32 public constant SET_COLLATERAL_ROLE = keccak256("WithdrawHook_setCollateral(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::23 => bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("WithdrawHook_setDepositRecord(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::24 => bytes32 public constant SET_WITHDRAWALS_ALLOWED_ROLE = keccak256("WithdrawHook_setWithdrawalsAllowed(bool)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::25 => bytes32 public constant SET_GLOBAL_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setGlobalPeriodLength(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::26 => bytes32 public constant SET_USER_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setUserPeriodLength(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::27 => bytes32 public constant SET_GLOBAL_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setGlobalWithdrawLimitPerPeriod(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::28 => bytes32 public constant SET_USER_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setUserWithdrawLimitPerPeriod(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::29 => bytes32 public constant SET_TREASURY_ROLE = keccak256("WithdrawHook_setTreasury(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::30 => bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("WithdrawHook_setTokenSender(ITokenSender)");
```

## [N-6] Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

```
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::73 => function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::22 => function depositAndTrade(uint256 baseTokenAmount, Permit calldata baseTokenPermit, Permit calldata collateralPermit, OffChainTradeParams calldata tradeParams) external override {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::24 => IERC20Permit(address(_baseToken)).permit(msg.sender, address(this), type(uint256).max, baseTokenPermit.deadline, baseTokenPermit.v, baseTokenPermit.r, baseTokenPermit.s);
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::32 => ISwapRouter.ExactInputSingleParams memory exactInputSingleParams = ISwapRouter.ExactInputSingleParams(address(_collateral), tradeParams.tokenOut, POOL_FEE_TIER, msg.sender, tradeParams.deadline, _collateralAmountMinted, tradeParams.amountOutMinimum, tradeParams.sqrtPriceLimitX96);
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::17 => function hook(address, uint256, uint256 _amountAfterFee) external view override { require(collateral.getReserve() - _amountAfterFee >= getMinReserve(), "reserve would fall below minimum"); }
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::41 => function getMinReserve() public view override returns (uint256) { return (depositRecord.getGlobalNetDepositAmount() * minReservePercentage) / PERCENT_DENOMINATOR; }
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::16 => function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders { require(_accountList.isIncluded(sender), "minter not allowed"); }
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::42 => constructor(address _governance, address _collateral, ILongShortToken _longToken, ILongShortToken _shortToken, uint256 _floorLongPayout, uint256 _ceilingLongPayout, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) {
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::22 => function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::28 => PrePOMarket _newMarket = new PrePOMarket{salt: _salt}(_governance, _collateral, ILongShortToken(address(_longToken)), ILongShortToken(address(_shortToken)), _floorLongPrice, _ceilingLongPrice, _floorValuation, _ceilingValuation, _expiryTime);
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::41 => function _createPairTokens(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 _longTokenSalt, bytes32 _shortTokenSalt) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {
```

