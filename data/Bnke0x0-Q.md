




### [L01] Missing checks for `address(0x0)` when assigning values to `address` state variables


#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::86 => manager = _newManager;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::92 => depositFee = _newDepositFee;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::98 => withdrawFee = _newWithdrawFee;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::103 => depositHook = _newDepositHook;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::108 => withdrawHook = _newWithdrawHook;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::113 => managerWithdrawHook = _newManagerWithdrawHook;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::55 => collateral = _newCollateral;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::60 => depositRecord = _newDepositRecord;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::65 => depositsAllowed = _newDepositsAllowed;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::24 => globalNetDepositCap = _newGlobalNetDepositCap;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::25 => userDepositCap = _newUserDepositCap;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::41 => globalNetDepositCap = _newGlobalNetDepositCap;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::46 => userDepositCap = _newUserDepositCap;
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::20 => collateral = _newCollateral;
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::25 => depositRecord = _newDepositRecord;
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::30 => require(_newMinReservePercentage <= PERCENT_DENOMINATOR, ">100%");
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::31 => minReservePercentage = _newMinReservePercentage;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::50 => longToken = _longToken;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::51 => shortToken = _shortToken;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::53 => floorLongPayout = _floorLongPayout;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::54 => ceilingLongPayout = _ceilingLongPayout;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::57 => floorValuation = _floorValuation;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::58 => ceilingValuation = _ceilingValuation;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::60 => expiryTime = _expiryTime;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::122 => finalLongPayout = _finalLongPayout;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::127 => require(_redemptionFee <= FEE_LIMIT, "Exceeds fee limit");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::128 => redemptionFee = _redemptionFee;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::82 => collateral = _newCollateral;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::87 => depositRecord = _newDepositRecord;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::92 => withdrawalsAllowed = _newWithdrawalsAllowed;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::97 => globalPeriodLength = _newGlobalPeriodLength;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::102 => userPeriodLength = _newUserPeriodLength;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::107 => globalWithdrawLimitPerPeriod = _newGlobalWithdrawLimitPerPeriod;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::112 => userWithdrawLimitPerPeriod = _newUserWithdrawLimitPerPeriod;
```



### [L02] `initialize` functions can be front-run

#### Impact
See [this](https://github.com/code-423n4/2021-10-badgerdao-findings/issues/40) finding from a prior badger-dao contest for details
#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::34 => function initialize(string memory _name, string memory _symbol) public initializer {
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::16 => function initialize() public initializer { OwnableUpgradeable.__Ownable_init(); }
```




### [L03] approve should be replaced with safeApprove or safeIncreaseAllowance() / safeDecreaseAllowance()

#### Impact
approve is subject to a known front-running attack. Consider using safeApprove instead:
#### Findings:
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



### [L04] `_safeMint()` should be used rather than `_mint()` wherever possible

#### Impact
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::58 => _mint(_recipient, _collateralMintAmount);
prepo-monorepo/apps/smart-contracts/core/contracts/LongShortToken.sol::11 => function mint(address _recipient, uint256 _amount) external onlyOwner { _mint(_recipient, _amount); }
```


### Non-Critical Issues

### [N01] Adding a return statement when the function defines a named return variable, is redundant


#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::60 => return _collateralMintAmount;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::73 => return _amount;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::125 => return collateral;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::129 => return depositRecord;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::133 => return globalPeriodLength;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::137 => return userPeriodLength;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::141 => return globalWithdrawLimitPerPeriod;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::145 => return userWithdrawLimitPerPeriod;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::149 => return lastGlobalPeriodReset;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::153 => return lastUserPeriodReset;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::157 => return globalAmountWithdrawnThisPeriod;
prepo-monorepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol::16 => return _accountList;
prepo-monorepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol::21 => return _allowedMsgSenders;
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::47 => return _requiredScore;
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::65 => return score;
prepo-monorepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol::17 => return _treasury;
prepo-monorepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol::26 => return _tokenSender;
```

### [N02] constants should be defined rather than using magic numbers


#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::19 => uint256 public constant FEE_DENOMINATOR = 1000000;
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::20 => uint256 public constant FEE_LIMIT = 100000;
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::12 => uint24 public constant override POOL_FEE_TIER = 10000;
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::12 => uint256 public constant PERCENT_DENOMINATOR = 1000000;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::29 => uint256 private constant MAX_PAYOUT = 1e18;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::30 => uint256 private constant FEE_DENOMINATOR = 1000000;
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::31 => uint256 private constant FEE_LIMIT = 100000;
```


### [N03] Variable names that consist of all capital letters should be reserved for `const`/`immutable `variables

#### Impact
If the variable needs to be different based on which class it comes from, a view/pure function should be used instead (e.g. like [this](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/76eee35971c2541585e05cbf258510dda7b2fbc6/contracts/token/ERC20/extensions/draft-IERC20Permit.sol#L59)).
#### Findings:
```
prepo-monorepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol::8 => IAccountList internal _accountList;
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::10 => uint256 internal _requiredScore;
prepo-monorepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol::8 => address internal _treasury;
prepo-monorepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol::9 => ITokenSender internal _tokenSender;
```


### [N04] Event is missing `indexed` fields

#### Impact
Each event should use three indexed fields if there are three or more fields
#### Findings:
```
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/ICollateral.sol::24 => event Deposit(address indexed depositor, uint256 amountAfterFee, uint256 fee);
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/ICollateral.sol::32 => event Withdraw(address indexed withdrawer, uint256 amountAfterFee, uint256 fee);
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol::24 => event CollectionScoresChange(IERC721[] collections, uint256[] scores);
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IPrePOMarket.sol::27 => event MarketCreated(address longToken, address shortToken, uint256 floorLongPayout, uint256 ceilingLongPayout, uint256 floorValuation, uint256 ceilingValuation, uint256 expiryTime);
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IPrePOMarket.sol::34 => event Mint(address indexed minter, uint256 amount);
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IPrePOMarket.sol::42 => event Redemption(address indexed redeemer, uint256 amountAfterFee, uint256 fee);
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IPrePOMarketFactory.sol::14 => event CollateralValidityChanged(address collateral, bool allowed);
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IPrePOMarketFactory.sol::19 => event MarketAdded(address market, bytes32 longShortHash);
```


### [N05] Open TODOs

#### Impact
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment
#### Findings:
```
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/IDepositRecord.sol::4 => // TODO interface copy needs some further edits to reflect new implementation
```





### [N06] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from public to external .
#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::34 => function initialize(string memory _name, string memory _symbol) public initializer {
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::69 => function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::71 => function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) { super.setRequiredScore(_newRequiredScore); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::73 => function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::75 => function removeCollections(IERC721[] memory _collections) public override onlyRole(REMOVE_COLLECTIONS_ROLE) { super.removeCollections(_collections); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::77 => function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) { super.setTreasury(_treasury); }
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::79 => function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); }
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::41 => function getMinReserve() public view override returns (uint256) { return (depositRecord.getGlobalNetDepositAmount() * minReservePercentage) / PERCENT_DENOMINATOR; }
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::18 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::20 => function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::16 => function initialize() public initializer { OwnableUpgradeable.__Ownable_init(); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::26 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::28 => function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::30 => function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::32 => function setTokenSender(ITokenSender tokenSender) public override onlyOwner { super.setTokenSender(tokenSender); }
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::116 => function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) {
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::120 => function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) {
prepo-monorepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol::10 => function setAccountList(IAccountList accountList) public virtual override {
prepo-monorepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol::15 => function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override {
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::17 => function setRequiredScore(uint256 requiredScore) public virtual override {
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::22 => function setCollectionScores(IERC721[] memory collections, uint256[] memory scores) public virtual override {
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::35 => function removeCollections(IERC721[] memory collections) public virtual override {
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::55 => function getAccountScore(address account) public view virtual override returns (uint256) {
prepo-monorepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol::11 => function setTreasury(address treasury) public virtual override {
prepo-monorepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol::20 => function setTokenSender(ITokenSender tokenSender) public virtual override {
```


### [N07] Constant redefined elsewhere

#### Impact
Consider defining in only one contract so that values cannot become out of sync when only one location is updated
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




