## [G-01] Using > 0 costs more gas than != 0 when used on a uint in a require() statement

When dealing with unsigned integer types, comparisons with != 0 are cheaper then with > 0. This change saves 6 gas per instance

```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::47 => if (depositFee > 0) { require(_fee > 0, "fee = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::48 => else { require(_amount > 0, "amount = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::67 => if (withdrawFee > 0) { require(_fee > 0, "fee = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::68 => else { require(_baseTokenAmount > 0, "amount = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::91 => if (redemptionFee > 0) { require(_expectedFee > 0, "fee = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::92 => else { require(_collateralAmount > 0, "amount = 0"); }
```

## [G-02] Use calldata instead of memory

Use calldata instead of memory for function parameters saves gas if the function argument is only read.

```
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::41 => function _createPairTokens(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 _longTokenSalt, bytes32 _shortTokenSalt) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {
```

## [G-03] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::13 => function set(address[] calldata _accounts, bool[] calldata _included) external override onlyOwner {
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::24 => function reset(address[] calldata _newIncludedAccounts) external override onlyOwner {
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::80 => function managerWithdraw(uint256 _amount) external override onlyRole(MANAGER_WITHDRAW_ROLE) nonReentrant {
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::85 => function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::90 => function setDepositFee(uint256 _newDepositFee) external override onlyRole(SET_DEPOSIT_FEE_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::96 => function setWithdrawFee(uint256 _newWithdrawFee) external override onlyRole(SET_WITHDRAW_FEE_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::102 => function setDepositHook(ICollateralHook _newDepositHook) external override onlyRole(SET_DEPOSIT_HOOK_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::107 => function setWithdrawHook(ICollateralHook _newWithdrawHook) external override onlyRole(SET_WITHDRAW_HOOK_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::112 => function setManagerWithdrawHook(ICollateralHook _newManagerWithdrawHook) external override onlyRole(SET_MANAGER_WITHDRAW_HOOK_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::43 => function hook(address _sender, uint256 _amountBeforeFee, uint256 _amountAfterFee) external override onlyCollateral {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::54 => function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::59 => function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::64 => function setDepositsAllowed(bool _newDepositsAllowed) external override onlyRole(SET_DEPOSITS_ALLOWED_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::69 => function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::71 => function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) { super.setRequiredScore(_newRequiredScore); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::73 => function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::75 => function removeCollections(IERC721[] memory _collections) public override onlyRole(REMOVE_COLLECTIONS_ROLE) { super.removeCollections(_collections); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::77 => function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) { super.setTreasury(_treasury); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::79 => function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::28 => function recordDeposit(address _sender, uint256 _amount) external override onlyAllowedHooks {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::35 => function recordWithdrawal(uint256 _amount) external override onlyAllowedHooks {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::40 => function setGlobalNetDepositCap(uint256 _newGlobalNetDepositCap) external override onlyRole(SET_GLOBAL_NET_DEPOSIT_CAP_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::45 => function setUserDepositCap(uint256 _newUserDepositCap) external override onlyRole(SET_USER_DEPOSIT_CAP_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::50 => function setAllowedHook(address _hook, bool _allowed) external override onlyRole(SET_ALLOWED_HOOK_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/LongShortToken.sol::11 => function mint(address _recipient, uint256 _amount) external onlyOwner { _mint(_recipient, _amount); }
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::19 => function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::24 => function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::29 => function setMinReservePercentage(uint256 _newMinReservePercentage) external override onlyRole(SET_MIN_RESERVE_PERCENTAGE_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::16 => function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders { require(_accountList.isIncluded(sender), "minter not allowed"); }
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::18 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::20 => function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::109 => function setMintHook(IMarketHook mintHook) external override onlyOwner {
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::114 => function setRedeemHook(IMarketHook redeemHook) external override onlyOwner {
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::119 => function setFinalLongPayout(uint256 _finalLongPayout) external override onlyOwner {
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::126 => function setRedemptionFee(uint256 _redemptionFee) external override onlyOwner {
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::22 => function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::36 => function setCollateralValidity(address _collateral, bool _validity) external override onlyOwner {
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::17 => function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders {
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::26 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::28 => function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::30 => function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::32 => function setTokenSender(ITokenSender tokenSender) public override onlyOwner { super.setTokenSender(tokenSender); }
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::36 => function send(address recipient, uint256 unconvertedAmount) external override onlyAllowedMsgSenders {
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::45 => function setPrice(IUintValue price) external override onlyRole(SET_PRICE_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::50 => function setPriceMultiplier(uint256 multiplier) external override onlyRole(SET_PRICE_MULTIPLIER_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::55 => function setScaledPriceLowerBound(uint256 lowerBound) external override onlyRole(SET_SCALED_PRICE_LOWER_BOUND_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::60 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public override onlyRole(SET_ALLOWED_MSG_SENDERS_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::81 => function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::86 => function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::91 => function setWithdrawalsAllowed(bool _newWithdrawalsAllowed) external override onlyRole(SET_WITHDRAWALS_ALLOWED_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::96 => function setGlobalPeriodLength(uint256 _newGlobalPeriodLength) external override onlyRole(SET_GLOBAL_PERIOD_LENGTH_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::101 => function setUserPeriodLength(uint256 _newUserPeriodLength) external override onlyRole(SET_USER_PERIOD_LENGTH_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::106 => function setGlobalWithdrawLimitPerPeriod(uint256 _newGlobalWithdrawLimitPerPeriod) external override onlyRole(SET_GLOBAL_WITHDRAW_LIMIT_PER_PERIOD_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::111 => function setUserWithdrawLimitPerPeriod(uint256 _newUserWithdrawLimitPerPeriod) external override onlyRole(SET_USER_WITHDRAW_LIMIT_PER_PERIOD_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::116 => function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::120 => function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) {
```

## [G-04] Using bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.
Use uint256(1) and uint256(2) for true/false instead

```
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::9 => mapping(uint256 => mapping(address => bool)) private _resetIndexToAccountToIncluded;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::15 => bool public override depositsAllowed;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::12 => mapping(address => bool) private allowedHooks;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::13 => mapping(address => bool) private validCollateral;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::12 => bool public override withdrawalsAllowed;
```

## [G-05] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, for example when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop

```
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::19 => ++i;
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::25 => resetIndex++;
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::30 => ++i;
```

## [G-06] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

use <x> = <x> + <y> or <x> = <x> - <y> instead to save gas

```
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::31 => globalNetDepositAmount += _amount;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::32 => userToDeposits[_sender] += _amount;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::36 => if (globalNetDepositAmount > _amount) { globalNetDepositAmount -= _amount; }
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::64 => globalAmountWithdrawnThisPeriod += _amountBeforeFee;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::71 => userToAmountWithdrawnThisPeriod[_sender] += _amountBeforeFee;
```

## [G-07] Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

```
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::14 => require(_accounts.length == _included.length, "Array length mismatch");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::47 => if (depositFee > 0) { require(_fee > 0, "fee = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::48 => else { require(_amount > 0, "amount = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::67 => if (withdrawFee > 0) { require(_fee > 0, "fee = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::68 => else { require(_baseTokenAmount > 0, "amount = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::91 => require(_newDepositFee <= FEE_LIMIT, "exceeds fee limit");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::97 => require(_newWithdrawFee <= FEE_LIMIT, "exceeds fee limit");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::28 => require(msg.sender == address(collateral), "msg.sender != collateral");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::44 => require(depositsAllowed, "deposits not allowed");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::45 => if (!_accountList.isIncluded(_sender)) require(_satisfiesScoreRequirement(_sender), "depositor not allowed");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::19 => require(allowedHooks[msg.sender], "msg.sender != allowed hook");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::29 => require(_amount + globalNetDepositAmount <= globalNetDepositCap, "Global deposit cap exceeded");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::30 => require(_amount + userToDeposits[_sender] <= userDepositCap, "User deposit cap exceeded");
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::17 => function hook(address, uint256, uint256 _amountAfterFee) external view override { require(collateral.getReserve() - _amountAfterFee >= getMinReserve(), "reserve would fall below minimum"); }
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::30 => require(_newMinReservePercentage <= PERCENT_DENOMINATOR, ">100%");
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::16 => function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders { require(_accountList.isIncluded(sender), "minter not allowed"); }
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::43 => require(_ceilingLongPayout > _floorLongPayout, "Ceiling must exceed floor");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::44 => require(_expiryTime > block.timestamp, "Invalid expiry");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::45 => require(_ceilingLongPayout <= MAX_PAYOUT, "Ceiling cannot exceed 1");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::66 => require(finalLongPayout > MAX_PAYOUT, "Market ended");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::67 => require(collateral.balanceOf(msg.sender) >= _amount, "Insufficient collateral");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::77 => require(longToken.balanceOf(msg.sender) >= _longAmount, "Insufficient long tokens");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::78 => require(shortToken.balanceOf(msg.sender) >= _shortAmount, "Insufficient short tokens");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::85 => require(_longAmount == _shortAmount, "Long and Short must be equal");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::91 => if (redemptionFee > 0) { require(_expectedFee > 0, "fee = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::92 => else { require(_collateralAmount > 0, "amount = 0"); }
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::120 => require(_finalLongPayout >= floorLongPayout, "Payout cannot be below floor");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::121 => require(_finalLongPayout <= ceilingLongPayout, "Payout cannot exceed ceiling");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::127 => require(_redemptionFee <= FEE_LIMIT, "Exceeds fee limit");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::23 => require(validCollateral[_collateral], "Invalid collateral");
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::18 => require(_accountList.isIncluded(sender), "redeemer not allowed");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::33 => require(msg.sender == address(collateral), "msg.sender != collateral");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::58 => require(withdrawalsAllowed, "withdrawals not allowed");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::63 => require(globalAmountWithdrawnThisPeriod + _amountBeforeFee <= globalWithdrawLimitPerPeriod, "global withdraw limit exceeded");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::70 => require(userToAmountWithdrawnThisPeriod[_sender] + _amountBeforeFee <= userWithdrawLimitPerPeriod, "user withdraw limit exceeded");
```

## [G-08] Use a more recent version of solidity

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath
Use a solidity version of at least 0.8.2 to get compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

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

## [G-09] Use bytes32 instead of string

Use bytes32 instead of string to save gas whenever possible. String is a dynamic data structure and therefore is more gas consuming then bytes32.

```
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::42 => string memory _longTokenName = string(abi.encodePacked("LONG", " ", _tokenNameSuffix));
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::43 => string memory _shortTokenName = string(abi.encodePacked("SHORT", " ", _tokenNameSuffix));
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::44 => string memory _longTokenSymbol = string(abi.encodePacked("L", "_", _tokenSymbolSuffix));
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::45 => string memory _shortTokenSymbol = string(abi.encodePacked("S", "_", _tokenSymbolSuffix));
```

## [G-10] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public and can save gas by doing so.

```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::34 => function initialize(string memory _name, string memory _symbol) public initializer {
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::16 => function initialize() public initializer { OwnableUpgradeable.__Ownable_init(); }
```

## [G-11] Not using the named return variables when a function returns, wastes deployment gas

It is not necessary to have both a named return and a return statement.

```
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::41 => function _createPairTokens(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 _longTokenSalt, bytes32 _shortTokenSalt) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {
```

## [G-12] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

```
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::11 => mapping(address => uint256) private userToDeposits;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::12 => mapping(address => bool) private allowedHooks;
```

## [G-13] internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::41 => function _createPairTokens(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 _longTokenSalt, bytes32 _shortTokenSalt) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {
```