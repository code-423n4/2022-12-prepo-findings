# QA Report
## Finding Summary
||Issue|Instances|
|-|-|-|
|[NC-01]|Long Lines (> 120 Characters)|45|
|[NC-02]|NatSpec Comment Missing Tags|38|
|[NC-03]|Events Not Indexed|33|
|[NC-04]|Order of Functions Not Compliant With Solidity Docs|18|
|[NC-05]|Style Guide Voiding Whitespace (parenthesis/brackets/braces)|8|
|[NC-06]|Power of Ten Literal > `10e3` Not In Scientific Notation|7|
|[NC-07]|`overriden` Typo|5|
|[NC-08]|Style Guide Voiding If Structure Braces / Inconsistent If Style|3|
|[NC-09]|`TODO` Left In Production Code|2|

### [NC-01] Long Lines (> 120 Characters)

Lines with greater length than 120 characters are used. The [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-lengthhttps://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length) suggests that all lines should be 120 characters or less in width.

#### Findings:

*/apps/smart-contracts/core/contracts/Collateral.sol*
Links: [9](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L9), [27](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L27), [112](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L112).
```solidity
9:	contract Collateral is ICollateral, ERC20PermitUpgradeable, SafeAccessControlEnumerableUpgradeable, ReentrancyGuardUpgradeable {
27:	  bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");
112:	  function setManagerWithdrawHook(ICollateralHook _newManagerWithdrawHook) external override onlyRole(SET_MANAGER_WITHDRAW_HOOK_ROLE) {
```

*/apps/smart-contracts/core/contracts/DepositHook.sol*
Links: [12](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L12), [22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L22), [69](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L69), [71](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L71), [73](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L73), [75](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L75), [79](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L79).
```solidity
12:	contract DepositHook is IDepositHook, AccountListCaller, NFTScoreRequirement, TokenSenderCaller, SafeAccessControlEnumerable {
22:	  bytes32 public constant SET_COLLECTION_SCORES_ROLE = keccak256("DepositHook_setCollectionScores(IERC721[],uint256[])");
69:	  function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) { super.setAccountList(accountList); }
71:	  function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) { super.setRequiredScore(_newRequiredScore); }
73:	  function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
75:	  function removeCollections(IERC721[] memory _collections) public override onlyRole(REMOVE_COLLECTIONS_ROLE) { super.removeCollections(_collections); }
79:	
  function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); }
```

*/apps/smart-contracts/core/contracts/DepositRecord.sol*
Links: [40](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L40), [61](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L61).
```solidity
40:	  function setGlobalNetDepositCap(uint256 _newGlobalNetDepositCap) external override onlyRole(SET_GLOBAL_NET_DEPOSIT_CAP_ROLE) {
61:	  function getUserDepositAmount(address _account) external view override returns (uint256) { return userToDeposits[_account]; }
```

*/apps/smart-contracts/core/contracts/DepositTradeHelper.sol*
Links: [22](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L22), [24](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L24), [28](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L28), [32](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L32).
```solidity
22:	  function depositAndTrade(uint256 baseTokenAmount, Permit calldata baseTokenPermit, Permit calldata collateralPermit, OffChainTradeParams calldata tradeParams) external override {
24:	      IERC20Permit(address(_baseToken)).permit(msg.sender, address(this), type(uint256).max, baseTokenPermit.deadline, baseTokenPermit.v, baseTokenPermit.r, baseTokenPermit.s);
28:	      _collateral.permit(msg.sender, address(this), type(uint256).max, collateralPermit.deadline, collateralPermit.v, collateralPermit.r, collateralPermit.s);
32:	    ISwapRouter.ExactInputSingleParams memory exactInputSingleParams = ISwapRouter.ExactInputSingleParams(address(_collateral), tradeParams.tokenOut, POOL_FEE_TIER, msg.sender, tradeParams.deadline, _collateralAmountMinted, tradeParams.amountOutMinimum, tradeParams.sqrtPriceLimitX96);
```

*/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol*
Links: [15](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L15), [17](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L17), [29](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L29), [41](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L41).
```solidity
15:	  bytes32 public constant SET_MIN_RESERVE_PERCENTAGE_ROLE = keccak256("ManagerWithdrawHook_setMinReservePercentage(uint256)");
17:	  function hook(address, uint256, uint256 _amountAfterFee) external view override { require(collateral.getReserve() - _amountAfterFee >= getMinReserve(), "reserve would fall below minimum"); }
29:	  function setMinReservePercentage(uint256 _newMinReservePercentage) external override onlyRole(SET_MIN_RESERVE_PERCENTAGE_ROLE) {
41:	  function getMinReserve() public view override returns (uint256) { return (depositRecord.getGlobalNetDepositAmount() * minReservePercentage) / PERCENT_DENOMINATOR; }
```

*/apps/smart-contracts/core/contracts/MintHook.sol*
Links: [16](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L16), [18](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L18), [20](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L20).
```solidity
16:	  function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders { require(_accountList.isIncluded(sender), "minter not allowed"); }
18:	  function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
20:	  function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
```

*/apps/smart-contracts/core/contracts/PrePOMarket.sol*
Links: [42](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L42), [62](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L62).
```solidity
42:	  constructor(address _governance, address _collateral, ILongShortToken _longToken, ILongShortToken _shortToken, uint256 _floorLongPayout, uint256 _ceilingLongPayout, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) {
62:	    emit MarketCreated(address(_longToken), address(_shortToken), _floorLongPayout, _ceilingLongPayout, _floorValuation, _ceilingValuation, _expiryTime);
```

*/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol*
Links: [18](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L18), [20](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L20), [22](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22), [25](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L25), [28](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L28), [41](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41).
```solidity
18:	  function isCollateralValid(address _collateral) external view override returns (bool) { return validCollateral[_collateral]; }
20:	  function getMarket(bytes32 _longShortHash) external view override returns (IPrePOMarket) { return IPrePOMarket(deployedMarkets[_longShortHash]); }
22:	  function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
25:	    (LongShortToken _longToken, LongShortToken _shortToken) = _createPairTokens(_tokenNameSuffix, _tokenSymbolSuffix, longTokenSalt, shortTokenSalt);
28:	    PrePOMarket _newMarket = new PrePOMarket{salt: _salt}(_governance, _collateral, ILongShortToken(address(_longToken)), ILongShortToken(address(_shortToken)), _floorLongPrice, _ceilingLongPrice, _floorValuation, _ceilingValuation, _expiryTime);
41:	  function _createPairTokens(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 _longTokenSalt, bytes32 _shortTokenSalt) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {
```

*/apps/smart-contracts/core/contracts/RedeemHook.sol*
Links: [17](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L17), [26](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L26), [28](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L28).
```solidity
17:	  function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders {
26:	  function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
28:	  function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
```

*/apps/smart-contracts/core/contracts/TokenSender.sol*
Links: [28](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L28), [60](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L60).
```solidity
28:	  bytes32 public constant SET_SCALED_PRICE_LOWER_BOUND_ROLE = keccak256("TokenSender_setScaledPriceLowerBound(uint256)");
60:	  function setAllowedMsgSenders(IAccountList allowedMsgSenders) public override onlyRole(SET_ALLOWED_MSG_SENDERS_ROLE) {
```

*/apps/smart-contracts/core/contracts/WithdrawHook.sol*
Links: [27](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L27), [28](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L28), [63](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L63), [70](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L70), [91](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L91), [96](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L96), [106](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L106), [111](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L111).
```solidity
27:	  bytes32 public constant SET_GLOBAL_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setGlobalWithdrawLimitPerPeriod(uint256)");
28:	  bytes32 public constant SET_USER_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setUserWithdrawLimitPerPeriod(uint256)");
63:	      require(globalAmountWithdrawnThisPeriod + _amountBeforeFee <= globalWithdrawLimitPerPeriod, "global withdraw limit exceeded");
70:	      require(userToAmountWithdrawnThisPeriod[_sender] + _amountBeforeFee <= userWithdrawLimitPerPeriod, "user withdraw limit exceeded");
91:	  function setWithdrawalsAllowed(bool _newWithdrawalsAllowed) external override onlyRole(SET_WITHDRAWALS_ALLOWED_ROLE) {
96:	  function setGlobalPeriodLength(uint256 _newGlobalPeriodLength) external override onlyRole(SET_GLOBAL_PERIOD_LENGTH_ROLE) {
106:	 function setGlobalWithdrawLimitPerPeriod(uint256 _newGlobalWithdrawLimitPerPeriod) external override onlyRole(SET_GLOBAL_WITHDRAW_LIMIT_PER_PERIOD_ROLE) {
111:	  function setUserWithdrawLimitPerPeriod(uint256 _newUserWithdrawLimitPerPeriod) external override onlyRole(SET_USER_WITHDRAW_LIMIT_PER_PERIOD_ROLE) {
```

*/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol*
Links: [27](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L27).
```solidity
27:	  event MarketCreated(address longToken, address shortToken, uint256 floorLongPayout, uint256 ceilingLongPayout, uint256 floorValuation, uint256 ceilingValuation, uint256 expiryTime);
```

### [NC-02] NatSpec Comment Missing Tags

Although tagless NatSpec comments are assumed to be `@notice` tagged it is best practice to explicitly write tags for empty comments.

*/apps/smart-contracts/core/contracts/PrePOMarket.sol*
Links: [33-41](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L33-L41).
```solidity
33:	/**
34:	* Assumes `_collateral`, `_longToken`, and `_shortToken` are
...
39:	* transferred to this contract via `createMarket()` in
40:	* `PrePOMarketFactory.sol`.
```

*apps/smart-contracts/core/contracts/DepositHook.sol*
Links: [36](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L36), [40](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L40).
```solidity
36:	* Records the deposit within `depositRecord`, and sends the fee to the
40:	* Uses `_amountAfterFee` for updating global net deposits since the fee is
```

*/apps/smart-contracts/core/contracts/WithdrawHook.sol*
Links: [45](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L45), [49](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L49).
```solidity
45:	* Records the deposit within `depositRecord`, and sends the fee to the
49:	* Uses `_amountAfterFee` for updating global net deposits since the fee is
```

*/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol*
Links: [40](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol).
```solidity
40:	* This function is meant to be overriden and does not include any
```

*/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol*
Links: [76](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L76), [79](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L79), [82](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L82), [84](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L84), [97](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L97), [100](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L100), [111](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L111).
```solidity
76:	* An optional external hook `depositHook`, is called to provide expanded
79:	* Fees are not directly captured, but approved to the `depositHook` for
82:	* Assumes Base Token approval has already been given by `msg.sender`.
84:	* Does not allow deposit amounts small enough to result in
97:	* Fees are not directly captured, but approved to the `withdrawHook` for
100:	* Does not allow withdraw amounts small enough to result in
111:	* Only callable by `manager`
```

*/apps/smart-contracts/core/contracts/interfaces/ICollateralHook.sol*
Links: [21](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateralHook.sol#L21), [24](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateralHook.sol#L24), [27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateralHook.sol#L27).
```solidity
21:	* `amountBeforeFee` is the Base Token amount deposited/withdrawn by the
24:	* `amountAfterFee` is the BaseToken amount deposited/withdrawn by the
27:	* Only callable by `collateral`.
```

*/apps/smart-contracts/core/contracts/interfaces/IDepositRecord.sol*
Links: [35](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositRecord.sol#L35), [45](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositRecord.sol#L45).
```solidity
35:	* Only callable by allowed hooks.
45:	* Only callable by allowed hooks.
```

*/apps/smart-contracts/core/contracts/interfaces/IDepositTradeHelper.sol*
Links: [46](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositTradeHelper.sol#L46), [51](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositTradeHelper.sol#L51), [53](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositTradeHelper.sol#L53).
```solidity
46:	* Approvals to both take BaseToken from the user and the Collateral
51:	* The LongShortToken to be purchased must be specified within
53:	* The pool to swap with will be automatically routed to using Uniswap's
```

*/apps/smart-contracts/core/contracts/interfaces/ILongShortToken.sol*
Links: [9](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ILongShortToken.sol#L9).
```solidity
9:	* The token can represent either a Long or Short position for the
```

*/apps/smart-contracts/core/contracts/interfaces/IManagerWithdrawHook.sol*
Links: [22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IManagerWithdrawHook.sol#L22).
```solidity
22:	* Must be a 4 decimal place percentage value e.g. 4.9999% = 49999.
```

*/apps/smart-contracts/core/contracts/interfaces/IMarketHook.sol*
Links: [13](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IMarketHook.sol#L13), [16](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IMarketHook.sol#L16), [20](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IMarketHook.sol#L20).
```solidity
13:	* `amountBeforeFee` is the Collateral amount used to mint a market position
16:	* `amountBeforeFee` is the Collateral amount used to mint a market position
20:	* Only callable by allowed `PrePOMarket` contracts.
```

*/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol*
Links: [73](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L73), [87](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L87), [90](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L90), [116](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L116).
```solidity
73:	* Minting will only be done by the team, and thus relies on the `_mintHook`
87:	* After the market has ended, users can redeem any amount of
90:	* A fee will be taken based on the `redemptionFee` factor.
116:	* Only callable by `owner()`.
```

*/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol*
Links: [27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol#L27), [30](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol#L30), [33](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol#L33), [36](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol#L36).
```solidity
27:	* Token names are generated from `tokenNameSuffix` as the name
30:	* "LONG "/"SHORT " are appended to respective names, "L_"/"S_" are
33:	* e.g. preSTRIPE 100-200 30-September 2021 =>
36:	* e.g. preSTRIPE_100-200_30SEP21 => L_preSTRIPE_100-200_30SEP21.
```

*/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol*
Links: [57](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol#L57), [71](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol#L71), [84](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol#L84), [96](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol#L96).
```solidity
57:	* If the global period changes while an existing period is ongoing, the
71:	* If the user period changes while an existing period is ongoing, the
84:	* If the global withdraw limit changes, the global total will be
96:	* If the user withdraw limit changes, account totals will be assessed based
```

### [NC-03] Events Not Indexed

It is best practice to have 3 indexed fields in events. Indexed fields help off-chain tools analyze on-chain activity through filtering these indexed fields.

#### Findings:

*/packages/prepo-shared-contracts/contracts/interfaces/IAccountListCaller.sol*
Links: [15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/IAccountListCaller.sol#L15).
```solidity
15:	event AccountListChange(IAccountList accountList);
```

*/packages/prepo-shared-contracts/contracts/interfaces/IAllowedMsgSenders.sol*
Links: [19](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/IAllowedMsgSenders.sol#L19).
```solidity
19:	event AllowedMsgSendersChange(IAccountList allowedMsgSenders);
```

*/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol*
Links: [17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol#L17).
```solidity
17:	event RequiredScoreChange(uint256 score);
```

*/packages/prepo-shared-contracts/contracts/interfaces/ITokenSender.sol*
Links: [17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/ITokenSender.sol#L17), [23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/ITokenSender.sol#L23), [29](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/ITokenSender.sol#L29).
```solidity
17:	event PriceChange(IUintValue price);
23:	event PriceMultiplierChange(uint256 priceMultiplier);
29:	event ScaledPriceLowerBoundChange(uint256 scaledPrice);
```

*/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol*
Links: [17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol#L17), [23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol#L23).
```solidity
17:	event TreasuryChange(address treasury);
23:	event TokenSenderChange(address tokenSender);
```

*/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol*
Links: [38](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L38), [44](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L44), [50](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L50), [56](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L56), [62](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L62), [68](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L68).
```solidity
38:	event ManagerChange(address manager);
44:	event DepositFeeChange(uint256 fee);
50:	event WithdrawFeeChange(uint256 fee);
56:	event DepositHookChange(address hook);
62:	event WithdrawHookChange(address hook);
68:	event ManagerWithdrawHookChange(address hook);
```

*/apps/smart-contracts/core/contracts/interfaces/ICollateralHook.sol*
Links: [15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/ICollateralHook.sol#L15).
```solidity
15:	event CollateralChange(address collateral);
```

*/apps/smart-contracts/core/contracts/interfaces/IDepositHook.sol*
Links: [15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositHook.sol#L15).
```solidity
15:	event DepositsAllowedChange(bool allowed);
```

*/apps/smart-contracts/core/contracts/interfaces/IDepositRecord.sol*
Links: [15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositRecord.sol#L15), [21](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositRecord.sol#L21), [28](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositRecord.sol#L28).
```solidity
15:	event GlobalNetDepositCapChange(uint256 cap);
21:	event UserDepositCapChange(uint256 cap);
28:	event AllowedHooksChange(address hook, bool allowed);
```

*/apps/smart-contracts/core/contracts/interfaces/IDepositRecordHook.sol*
Links: [16](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositRecordHook.sol#L16).
```solidity
16:	event DepositRecordChange(address depositRecord);
```

*/apps/smart-contracts/core/contracts/interfaces/IManagerWithdrawHook.sol*
Links: [16](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IManagerWithdrawHook.sol#L16).
```solidity
16:	event MinReservePercentageChange(uint256 percentage);
```

*/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol*
Links: [27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L27), [48](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L48), [54](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L54), [60](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L60), [66](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L66).
```solidity
27:	event MarketCreated(address longToken, address shortToken, uint256 floorLongPayout, uint256 ceilingLongPayout, uint256 floorValuation, uint256 ceilingValuation, uint256 expiryTime);
48:	event MintHookChange(address hook);
54:	event RedeemHookChange(address hook);
60:	event FinalLongPayoutSet(uint256 payout);
66:	event RedemptionFeeChange(uint256 fee);
```

*/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol*
Links: [14](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol#L14), [19](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol#L19).
```solidity
14:	event CollateralValidityChanged(address collateral, bool allowed);
19:	event MarketAdded(address market, bytes32 longShortHash);
```

*/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol*
Links: [17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol#L17), [23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol#L23), [29](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol#L29), [35](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol#L35), [41](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol#L41).
```solidity
17:	event WithdrawalsAllowedChange(bool allowed);
23:	event GlobalPeriodLengthChange(uint256 period);
29:	event UserPeriodLengthChange(uint256 period);
35:	event GlobalWithdrawLimitPerPeriodChange(uint256 limit);
41:	event UserWithdrawLimitPerPeriodChange(uint256 limit);
```

### [NC-04] Order of Functions Not Compliant With Solidity Docs

The [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) suggests the following function ordering:  constructor, receive function (if exists), fallback function (if exists), external, public, internal, private.

#### Findings:

The following contracts are not compliant (examples are only to prove the functions are out of order NOT a full description): 

[AccountListCaller.sol](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol): external functions are positioned after public functions.
[AllowedMsgSenders.sol](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol): external functions are positioned after public functions.
[TokenSenderCaller.sol](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol): external functions are positioned after public functions.
[Collateral.sol](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol): external functions are positioned after public functions.
[DepositHook.sol](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol): external functions are positioned after public functions.
[PrePOMarketFactory.sol](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol): external functions are positioned after public functions.
[TokenSender.sol](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol): external functions are positioned after public functions.
[WithdrawHook.sol](https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol): external functions are positioned after public functions.

### [NC-05] Style Guide Voiding Whitespace (parenthesis/brackets/braces)

The [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#whitespace-in-expressions) recommends to *"[a]void extraneous whitespace [i]mmediately inside parenthesis, brackets or braces, with the exception of single line function declarations"*. There exists for loop pre-bracket trailing spaces in the codebase.

#### Findings:

*/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol*
Links: [25](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L25), [37](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L37), [58](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L58).
```solidity
25:	for (uint256 i = 0; i < numCollections; ) {
37:	for (uint256 i = 0; i < numCollections; ) {
58:	for (uint256 i = 0; i < numCollections; ) {
```

### [NC-06] Power of Ten Literal > `10e3` Not In Scientific Notation

Power of ten literals > `10e3` are easier to read when expressed in scientific notation. Consider expressing large powers of ten in scientific notation (Ex. 10e5).

#### Findings:

*/apps/smart-contracts/core/contracts/Collateral.sol*
Links: [19](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L19), [20](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L20).
```solidity
19:	uint256 public constant FEE_DENOMINATOR = 1000000;
20:	uint256 public constant FEE_LIMIT = 100000;
```

*/apps/smart-contracts/core/contracts/DepositTradeHelper.sol*
Links: [12](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L12).
```solidity
12:	uint24 public constant override POOL_FEE_TIER = 10000;
```

*/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol*
Links: [12](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L12).
```solidity
12:	uint256 public constant PERCENT_DENOMINATOR = 1000000;
```

*/apps/smart-contracts/core/contracts/PrePOMarket.sol*
Links: [30](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L30), [31](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L31).
```solidity
30:	uint256 private constant FEE_DENOMINATOR = 1000000;
31:	uint256 private constant FEE_LIMIT = 100000;
```
  
### [NC-07] `overriden` Typo

The word `overridden` is misspelled as `overriden`.

#### Findings:

*/packages/prepo-shared-contracts/contracts/interfaces/IAccountListCaller.sol*
Links: [19](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/IAccountListCaller.sol#L19).
```solidity
19:	* @dev This function is meant to be overriden and does not include any
```

*/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol*
Links: [28](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol#L28), [40](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol#L40), [49](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol#L49).
```solidity
28:	* @dev This function is meant to be overriden and does not include any
40:	* This function is meant to be overriden and does not include any
49:	* @dev This function is meant to be overriden and does not include any
```

*/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol*
Links: [27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol#L27).
```solidity
27:	* @dev This function is meant to be overriden and does not include any
```
  
### [NC-08] Style Guide Voiding If Structure Braces
  
  The [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#control-structures) states: *[t]he braces denoting the body of a contract, library, functions and structs should ... close on their own line at the same indentation level as the beginning of the declaration*. There are a few places in the codebase that add braces for single line if/else statements. There are places in the codebase that implement single line if statements without braces. Consider sticking to a single style (prefferably the [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#control-structures)).
  
  #### Findings:
  
  */apps/smart-contracts/core/contracts/Collateral.sol*
  Links: [47-48](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L47-L48), [67-68](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L67-L68).
  ```solidity
47:	if (depositFee > 0) { require(_fee > 0, "fee = 0"); }
48:	else { require(_amount > 0, "amount = 0"); }
```
 ```solidity
67:	if (withdrawFee > 0) { require(_fee > 0, "fee = 0"); }
68:	else { require(_baseTokenAmount > 0, "amount = 0"); }
```

*/apps/smart-contracts/core/contracts/PrePOMarket.sol*
Links: [91-92](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L91-L92).
  ```solidity
91:	if (redemptionFee > 0) { require(_expectedFee > 0, "fee = 0"); }
92:	else { require(_collateralAmount > 0, "amount = 0"); }
```

### [NC-09] `TODO` Left In Production Code

`TODO`s should not be in production code. Each `TODO` should either be discarded or implemented if needed prior to production.

#### Findings:

*/apps/smart-contracts/core/contracts/interfaces/IDepositRecord.sol*
Links: [4](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/interfaces/IDepositRecord.sol#L4).

```solidity
4:	// TODO interface copy needs some further edits to reflect new implementation
```

*/apps/smart-contracts/core/contracts/TokenSender.sol*
Links: [15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L15).
```solidity
15:	  WithdrawERC20, // TODO: Access control when WithdrawERC20 updated
```