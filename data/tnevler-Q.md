# Report
## Non-Critical Issues ##
### [N-1]: Use scientific notation
**Context:** 
```
apps\smart-contracts\core\contracts\Collateral.sol:
  19:   uint256 public constant FEE_DENOMINATOR = 1000000; 
  20:   uint256 public constant FEE_LIMIT = 100000;
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L19 

```
apps\smart-contracts\core\contracts\DepositTradeHelper.sol:
  12:   uint24 public constant override POOL_FEE_TIER = 10000; 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L12 

```
apps\smart-contracts\core\contracts\ManagerWithdrawHook.sol:
  12:   uint256 public constant PERCENT_DENOMINATOR = 1000000; 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L12

```
apps\smart-contracts\core\contracts\PrePOMarket.sol:
  30:   uint256 private constant FEE_DENOMINATOR = 1000000;
  31:   uint256 private constant FEE_LIMIT = 100000;
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L30

```
apps\smart-contracts\core\contracts\TokenSender.sol:
  25:   uint256 public constant MULTIPLIER_DENOMINATOR = 10000; 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L25

**Description:** 
Scientific notation should be used for better code readability.

### [N-2]: Event is missing
**Context:**
```
apps\smart-contracts\core\contracts\Collateral.sol:
  82:     baseToken.transfer(manager, _amount);
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L82

```
apps\smart-contracts\core\contracts\DepositHook.sol:
  50:       _tokenSender.send(_sender, _fee); 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L50

```
apps\smart-contracts\core\contracts\WithdrawHook.sol:
  77:       _tokenSender.send(_sender, _fee); 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L77

### [N-3]: Function defines a named return variable but then it uses return statements
**Context:**
```
apps\smart-contracts\core\contracts\PrePOMarketFactory.sol:
  47      _newShortToken = new LongShortToken{salt: _shortTokenSalt}(_shortTokenName, _shortTokenSymbol);
  48:     return (_newLongToken, _newShortToken);
  49    }
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L48

**Recommendation:**
Choose named return variable or return statement. It is unnecessary to use both.

### [N-4]: Wrong order of functions  
**Context:**
```
apps\smart-contracts\core\contracts\Collateral.sol:
  44     */
  45:   function deposit(address _recipient, uint256 _amount) external override nonReentrant returns (uint256) { 
  46      uint256 _fee = (_amount * depositFee) / FEE_DENOMINATOR;
```
(external function can not go after public function)
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L45

```
apps\smart-contracts\core\contracts\DepositHook.sol:
  80  
  81:   function getCollateral() external view override returns (ICollateral) { return collateral; } 
  82  
```
(external function can not go after public function)
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L81

```
apps\smart-contracts\core\contracts\ManagerWithdrawHook.sol:
  18  
  19:   function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) { 
  20      collateral = _newCollateral;
```
(external function can not go after external view function)
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L19 

```
apps\smart-contracts\core\contracts\PrePOMarketFactory.sol:
  17  
  18:   function isCollateralValid(address _collateral) external view override returns (bool) { return validCollateral[_collateral]; } 
  19  
```
(external function can not go after public function)
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L18

```
apps\smart-contracts\core\contracts\TokenSender.sol:
  63  
  64:   function getOutputToken() external view override returns (IERC20) { 
  65      return _outputToken;
```
(external function can not go after public function)
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L64

```
packages\prepo-shared-contracts\contracts\AccountListCaller.sol:
  14  
  15:   function getAccountList() external view override returns (IAccountList) { 
  16      return _accountList;
```
(external function can not go after public function)
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol#L15

```
packages\prepo-shared-contracts\contracts\AllowedMsgSenders.sol:
  19  
  20:   function getAllowedMsgSenders() external view virtual override returns (IAccountList) { 
  21      return _allowedMsgSenders;
```
(external function can not go after public function)
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol#L20

```
packages\prepo-shared-contracts\contracts\NFTScoreRequirement.sol:
  16  
  17:   function setRequiredScore(uint256 requiredScore) public virtual override { 
  18      _requiredScore = requiredScore;
```
(public function can not go after internal function)
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L17

```
packages\prepo-shared-contracts\contracts\TokenSenderCaller.sol:
  16:   function getTreasury() external view override returns (address) {
  17      return _treasury;
```
(external function can not go after public function)
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol#L16

### [N-5]: NatSpec is missing

Most contracts are poorly commented.

### [N-6]: Use immutable instead of constant with keccak expression 
**Context:** 
```
apps\smart-contracts\core\contracts\Collateral.sol:
  21:   bytes32 public constant MANAGER_WITHDRAW_ROLE = keccak256("Collateral_managerWithdraw(uint256)");
  22:   bytes32 public constant SET_MANAGER_ROLE = keccak256("Collateral_setManager(address)");
  23:   bytes32 public constant SET_DEPOSIT_FEE_ROLE = keccak256("Collateral_setDepositFee(uint256)"); 
  24:   bytes32 public constant SET_WITHDRAW_FEE_ROLE = keccak256("Collateral_setWithdrawFee(uint256)"); 
  25:   bytes32 public constant SET_DEPOSIT_HOOK_ROLE = keccak256("Collateral_setDepositHook(ICollateralHook)"); 
  26:   bytes32 public constant SET_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setWithdrawHook(ICollateralHook)");  
  27:   bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)"); 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L21

```
apps\smart-contracts\core\contracts\DepositHook.sol:
  17:   bytes32 public constant SET_COLLATERAL_ROLE = keccak256("DepositHook_setCollateral(address)");
  18:   bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("DepositHook_setDepositRecord(address)");
  19:   bytes32 public constant SET_DEPOSITS_ALLOWED_ROLE = keccak256("DepositHook_setDepositsAllowed(bool)");
  20:   bytes32 public constant SET_ACCOUNT_LIST_ROLE = keccak256("DepositHook_setAccountList(IAccountList)");
  21:   bytes32 public constant SET_REQUIRED_SCORE_ROLE = keccak256("DepositHook_setRequiredScore(uint256)");
  22:   bytes32 public constant SET_COLLECTION_SCORES_ROLE = keccak256("DepositHook_setCollectionScores(IERC721[],uint256[])");
  23:   bytes32 public constant REMOVE_COLLECTIONS_ROLE = keccak256("DepositHook_removeCollections(IERC721[])");
  24:   bytes32 public constant SET_TREASURY_ROLE = keccak256("DepositHook_setTreasury(address)");
  25:   bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("DepositHook_setTokenSender(ITokenSender)");
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L17

```
apps\smart-contracts\core\contracts\DepositRecord.sol:
  14:   bytes32 public constant SET_GLOBAL_NET_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setGlobalNetDepositCap(uint256)");
  15:   bytes32 public constant SET_USER_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setUserDepositCap(uint256)");
  16:   bytes32 public constant SET_ALLOWED_HOOK_ROLE = keccak256("DepositRecord_setAllowedHook(address)");
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L14

```
apps\smart-contracts\core\contracts\ManagerWithdrawHook.sol:
  13:   bytes32 public constant SET_COLLATERAL_ROLE = keccak256("ManagerWithdrawHook_setCollateral(address)");
  14:   bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("ManagerWithdrawHook_setDepositRecord(address)");
  15:   bytes32 public constant SET_MIN_RESERVE_PERCENTAGE_ROLE = keccak256("ManagerWithdrawHook_setMinReservePercentage(uint256)"); 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L13

```
apps\smart-contracts\core\contracts\TokenSender.sol:
  26:   bytes32 public constant SET_PRICE_ROLE = keccak256("TokenSender_setPrice(IUintValue)");
  27:   bytes32 public constant SET_PRICE_MULTIPLIER_ROLE = keccak256("TokenSender_setPriceMultiplier(uint256)");
  28:   bytes32 public constant SET_SCALED_PRICE_LOWER_BOUND_ROLE = keccak256("TokenSender_setScaledPriceLowerBound(uint256)");
  29:   bytes32 public constant SET_ALLOWED_MSG_SENDERS_ROLE = keccak256("TokenSender_setAllowedMsgSenders(IAccountList)"); 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L26

```
apps\smart-contracts\core\contracts\WithdrawHook.sol:
  22:   bytes32 public constant SET_COLLATERAL_ROLE = keccak256("WithdrawHook_setCollateral(address)");
  23:   bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("WithdrawHook_setDepositRecord(address)");
  24:   bytes32 public constant SET_WITHDRAWALS_ALLOWED_ROLE = keccak256("WithdrawHook_setWithdrawalsAllowed(bool)");
  25:   bytes32 public constant SET_GLOBAL_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setGlobalPeriodLength(uint256)");
  26:   bytes32 public constant SET_USER_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setUserPeriodLength(uint256)");
  27:   bytes32 public constant SET_GLOBAL_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setGlobalWithdrawLimitPerPeriod(uint256)");
  28:   bytes32 public constant SET_USER_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setUserWithdrawLimitPerPeriod(uint256)");
  29:   bytes32 public constant SET_TREASURY_ROLE = keccak256("WithdrawHook_setTreasury(address)");
  30:   bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("WithdrawHook_setTokenSender(ITokenSender)");
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L22

**Description:** 
According to [official solidity documentation](https://docs.soliditylang.org/en/v0.8.17/contracts.html#constant-and-immutable-state-variables) for a constant variable, the expression assigned to it is copied to all the places where it is accessed and also re-evaluated each time. It is recommended to use immutable instead.

### [N-7]: TODO
**Context:** 
```
apps\smart-contracts\core\contracts\TokenSender.sol:
  14    AllowedMsgSenders,
  15:   WithdrawERC20, // TODO: Access control when WithdrawERC20 updated 
  16    SafeAccessControlEnumerable
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L15

### [N-8]: Line is too long
**Context:** 
```
apps\smart-contracts\core\contracts\Collateral.sol:
    9: contract Collateral is ICollateral, ERC20PermitUpgradeable, SafeAccessControlEnumerableUpgradeable, ReentrancyGuardUpgradeable { 

   27:   bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");

  112:   function setManagerWithdrawHook(ICollateralHook _newManagerWithdrawHook) external override onlyRole(SET_MANAGER_WITHDRAW_HOOK_ROLE) { 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L9

```
apps\smart-contracts\core\contracts\DepositHook.sol:
  12: contract DepositHook is IDepositHook, AccountListCaller, NFTScoreRequirement, TokenSenderCaller, SafeAccessControlEnumerable {

  69:   function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) { super.setAccountList(accountList); }

  71:   function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) { super.setRequiredScore(_newRequiredScore); } 
  
  73:   function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
  
  75:   function removeCollections(IERC721[] memory _collections) public override onlyRole(REMOVE_COLLECTIONS_ROLE) { super.removeCollections(_collections); }   

  79:   function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); } 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L12

```
apps\smart-contracts\core\contracts\DepositRecord.sol:
  40:   function setGlobalNetDepositCap(uint256 _newGlobalNetDepositCap) external override onlyRole(SET_GLOBAL_NET_DEPOSIT_CAP_ROLE) { 

  61:   function getUserDepositAmount(address _account) external view override returns (uint256) { return userToDeposits[_account]; } 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L40

```
apps\smart-contracts\core\contracts\DepositTradeHelper.sol:
  22:   function depositAndTrade(uint256 baseTokenAmount, Permit calldata baseTokenPermit, Permit calldata collateralPermit, OffChainTradeParams calldata tradeParams) external override { 

  24:       IERC20Permit(address(_baseToken)).permit(msg.sender, address(this), type(uint256).max, baseTokenPermit.deadline, baseTokenPermit.v, baseTokenPermit.r, baseTokenPermit.s);

  32:     ISwapRouter.ExactInputSingleParams memory exactInputSingleParams = ISwapRouter.ExactInputSingleParams(address(_collateral), tradeParams.tokenOut, POOL_FEE_TIER, msg.sender, tradeParams.deadline, _collateralAmountMinted, tradeParams.amountOutMinimum, tradeParams.sqrtPriceLimitX96);
 ```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L22
```
apps\smart-contracts\core\contracts\ManagerWithdrawHook.sol:
  15:   bytes32 public constant SET_MIN_RESERVE_PERCENTAGE_ROLE = keccak256("ManagerWithdrawHook_setMinReservePercentage(uint256)"); 
  
  17:   function hook(address, uint256, uint256 _amountAfterFee) external view override { require(collateral.getReserve() - _amountAfterFee >= getMinReserve(), "reserve would fall below minimum"); } 

  29:   function setMinReservePercentage(uint256 _newMinReservePercentage) external override onlyRole(SET_MIN_RESERVE_PERCENTAGE_ROLE) { 

  41:   function getMinReserve() public view override returns (uint256) { return (depositRecord.getGlobalNetDepositAmount() * minReservePercentage) / PERCENT_DENOMINATOR; } 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L15

```
apps\smart-contracts\core\contracts\MintHook.sol:
  16:   function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders { require(_accountList.isIncluded(sender), "minter not allowed"); }
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L16

```
apps\smart-contracts\core\contracts\PrePOMarket.sol:
  42:   constructor(address _governance, address _collateral, ILongShortToken _longToken, ILongShortToken _shortToken, uint256 _floorLongPayout, uint256 _ceilingLongPayout, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) { 
 
  62:     emit MarketCreated(address(_longToken), address(_shortToken), _floorLongPayout, _ceilingLongPayout, _floorValuation, _ceilingValuation, _expiryTime); 
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L42

```
apps\smart-contracts\core\contracts\PrePOMarketFactory.sol:
  18:   function isCollateralValid(address _collateral) external view override returns (bool) { return validCollateral[_collateral]; }
  
  20:   function getMarket(bytes32 _longShortHash) external view override returns (IPrePOMarket) { return IPrePOMarket(deployedMarkets[_longShortHash]); } 
   
  22:   function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {  
 
  25:     (LongShortToken _longToken, LongShortToken _shortToken) = _createPairTokens(_tokenNameSuffix, _tokenSymbolSuffix, longTokenSalt, shortTokenSalt); 

  28:     PrePOMarket _newMarket = new PrePOMarket{salt: _salt}(_governance, _collateral, ILongShortToken(address(_longToken)), ILongShortToken(address(_shortToken)), _floorLongPrice, _ceilingLongPrice, _floorValuation, _ceilingValuation, _expiryTime); 

  41:   function _createPairTokens(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 _longTokenSalt, bytes32 _shortTokenSalt) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L18

```
apps\smart-contracts\core\contracts\RedeemHook.sol:
  26:   function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
```
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L26

**Description:** 
Maximum suggested line length is [120 characters](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length).
