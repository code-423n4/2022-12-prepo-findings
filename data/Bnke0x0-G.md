

### [G01] State variables only set in the constructor should be declared `immutable`

#### Impact
Avoids a Gusset (20000 gas)
#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::12 => address private manager;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::13 => uint256 private depositFee;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::14 => uint256 private withdrawFee;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::15 => ICollateralHook private depositHook;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::16 => ICollateralHook private withdrawHook;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::17 => ICollateralHook private managerWithdrawHook;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::13 => ICollateral private collateral;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::14 => IDepositRecord private depositRecord;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::15 => bool public override depositsAllowed;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::8 => uint256 private globalNetDepositCap;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::9 => uint256 private globalNetDepositAmount;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::10 => uint256 private userDepositCap;
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::8 => ICollateral private collateral;
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::9 => IDepositRecord private depositRecord;
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::10 => uint256 private minReservePercentage;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::11 => IMarketHook private _mintHook;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::12 => IMarketHook private _redeemHook;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::20 => uint256 private finalLongPayout;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::25 => uint256 private redemptionFee;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::10 => ICollateral private collateral;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::11 => IDepositRecord private depositRecord;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::12 => bool public override withdrawalsAllowed;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::13 => uint256 private globalPeriodLength;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::14 => uint256 private userPeriodLength;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::15 => uint256 private globalWithdrawLimitPerPeriod;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::16 => uint256 private userWithdrawLimitPerPeriod;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::17 => uint256 private lastGlobalPeriodReset;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::18 => uint256 private lastUserPeriodReset;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::19 => uint256 private globalAmountWithdrawnThisPeriod;
prepo-monorepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol::8 => IAccountList private _allowedMsgSenders;
```



### [G02] `require()`/`revert()` strings longer than 32 bytes cost extra gas


#### Findings:
```
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::23 => require(collections.length == scores.length, "collections.length != scores.length");
```





### [G03] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement


#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::92 => else { require(_collateralAmount > 0, "amount = 0"); }
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::26 => require(scores[i] > 0, "score == 0");
```



### [G04] It costs more gas to initialize variables to zero than to let the default of zero be applied


#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::37 => else { globalNetDepositAmount = 0; }
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::99 => } else { _actualFee = 0; }
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::25 => for (uint256 i = 0; i < numCollections; ) {
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::37 => for (uint256 i = 0; i < numCollections; ) {
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::58 => for (uint256 i = 0; i < numCollections; ) {
```


### [G05] Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

#### Impact
See [this](https://github.com/ethereum/solidity/issues/9232) issue for a detailed description of the issue
#### Findings:
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



### [G06] Use custom errors rather than `revert()`/`require()` strings to save deployment gas


#### Findings:
```
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
prepo-monorepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol::11 => require(_allowedMsgSenders.isIncluded(msg.sender), "msg.sender not allowed");
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::14 => return _requiredScore == 0 || getAccountScore(account) >= _requiredScore;
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::17 => function setRequiredScore(uint256 requiredScore) public virtual override {
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::18 => _requiredScore = requiredScore;
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::23 => require(collections.length == scores.length, "collections.length != scores.length");
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::26 => require(scores[i] > 0, "score == 0");
```



### [G07] Functions guaranteed to revert when called by normal users can be marked `payable`

#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### Findings:
```
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



### [G08] Use a more recent version of solidity

#### Impact
Use a solidity version of at least `0.8.10` to have external calls skip
 contract existence checks if the external call has a return value
#### Findings:
```
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
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IAccountListCaller.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IAllowedMsgSenders.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/ICollateral.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/ICollateralHook.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IDepositHook.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IDepositRecord.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IDepositRecordHook.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IDepositTradeHelper.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/ILongShortToken.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IManagerWithdrawHook.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IMarketHook.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IPrePOMarket.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IPrePOMarketFactory.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/ITokenSender.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IUintValue.sol::2 => pragma solidity =0.8.7;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IWithdrawHook.sol::2 => pragma solidity =0.8.7;
```


### [G09] Use `calldata` instead of `memory` for function parameters

#### Impact
Use calldata instead of memory for function parameters. Having function arguments use calldata instead of memory can save gas.
#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::34 => function initialize(string memory _name, string memory _symbol) public initializer {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::73 => function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::75 => function removeCollections(IERC721[] memory _collections) public override onlyRole(REMOVE_COLLECTIONS_ROLE) { super.removeCollections(_collections); }
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::22 => function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::41 => function _createPairTokens(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 _longTokenSalt, bytes32 _shortTokenSalt) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::22 => function setCollectionScores(IERC721[] memory collections, uint256[] memory scores) public virtual override {
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::35 => function removeCollections(IERC721[] memory collections) public virtual override {
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol::43 => function setCollectionScores(IERC721[] memory collections, uint256[] memory scores) external;
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol::52 => function removeCollections(IERC721[] memory collections) external;
```



### [G10] Amounts should be checked for 0 before calling a transfer

#### Impact
Checking non-zero transfer values can avoid an expensive external call and save gas.

While this is done in some places, it’s not consistently done in the solution.

I suggest adding a non-zero-value check here:
#### Findings:
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
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::76 => collateral.getBaseToken().transferFrom(address(collateral), _treasury, _fee);
```



### [G11] Using `bools` for storage incurs overhead


#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::12 => mapping(address => bool) private allowedHooks;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::13 => mapping(address => bool) private validCollateral;
```





### [G12] Using `private` rather than `public` for constants, saves gas

#### Impact
If needed, the value can be read from the verified contract source 
code. Savings are due to the compiler not having to create non-payable getter functions for deployment call data, and not adding another entry to the method ID table
#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::19 => uint256 public constant FEE_DENOMINATOR = 1000000;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::20 => uint256 public constant FEE_LIMIT = 100000;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::12 => uint24 public constant override POOL_FEE_TIER = 10000;
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::12 => uint256 public constant PERCENT_DENOMINATOR = 1000000;
```






### [G13] Empty blocks should be removed or emit something

#### Impact
The code should be refactored such that they no longer exist, or the 
block should do something useful, such as emitting an event or 
reverting. If the contract is meant to be extended, the contract should 
be abstract and the function signatures be added without 
any default implementation. If the block is an empty if-statement block 
to avoid doing subsequent checks in the else-if/else conditions, the 
else-if/else conditions should be nested under the negation of the 
if-statement, because they involve different classes of checks, which 
may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}})
#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/LongShortToken.sol::9 => constructor(string memory name_, string memory symbol_) ERC20(name_, symbol_) {}
```





