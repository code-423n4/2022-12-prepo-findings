### Gas Optimizations List
| Number | Optimization Details | Context |
|:--:|:-------| :-----:|
| [G-01] | Remove the `initializer` modifier [20 K Gas Saved] | 2 |
| [G-02] |Storing the state variable by packing it | 1 |
| [G-03] | internal functions only called once can be inlined to save gas | 2 |
| [G-04] | Using `delete` statement can save gas  | 2 |
| [G-05] | Using storage instead of memory for structs/arrays saves gas | 7|
| [G-06] | Use ``assembly`` to write _address storage values_  | 23 |
| [G-07] | Setting the _constructor_ to `payable`  | 5 |
| [G-08] | Functions guaranteed to revert_ when callled by normal users can be marked `payable`  | 55 |
| [G-09] | State variables should be cached in stack variables rather than re-reading them from storage  | 7 |
| [G-10] | require() or revert() statements that check input arguments should be at the top of the function | 1 |
| [G-11] | Empty blocks should be removed or emit something  |1 |
| [G-12] | Optimize names to save gas  | All contracts |
| [G-13] |`x += y` costs more gas than `x = x + y` for state variables  |4  |
| [G-14] | It costs more gas to initialize non-constant/non-immutable variables to zero than to let the default of zero be applied  |3 |
| [G-15] | Comparison operators |18  |
| [G-16] | Use a more recent version of solidity |All contracts  |
| [G-17] | The solady Library's Ownable contract is significantly gas-optimized, which can be used |  |
| [G-18] | Use `Solmate SafeTransferLib ` contracts |  |

Total 18 issues

### Suggestions
| Number | Suggestion Details |
|:--:|:-------|
| [S-01] |Use `v4.8.0 OpenZeppelin` contracts |

### [G-01] Remove the `initializer` modifier

If we can just ensure that the `initialize()` function could only be called from within the constructor, we shouldn't need to worry about it getting called again. 

```solidity
2 results - 2 files

apps\smart-contracts\core\contracts\Collateral.sol:
  34:   function initialize(string memory _name, string memory _symbol) public initializer {

apps\smart-contracts\core\contracts\PrePOMarketFactory.sol:
  16:   function initialize() public initializer { OwnableUpgradeable.__Ownable_init(); }
```

In the EVM, the constructor's job is actually to return the bytecode that will live at the contract's address. So, while inside a constructor, your address `(address(this))` will be the deployment address, but there will be no bytecode at that address! So if we check `address(this).code.length` before the constructor has finished, even from within a delegatecall, we will get 0. So now let's update our `initialize()` function to only run if we are inside a constructor:

```diff
apps\smart-contracts\core\contracts\Collateral.sol:

  34:   function initialize(string memory _name, string memory _symbol) public initializer {
+          require(address(this).code.length == 0, 'not in constructor’);

```

Now the Proxy contract's constructor can still delegatecall initialize(), but if anyone attempts to call it again (after deployment) through the Proxy instance, or tries to call it directly on the above instance, it will revert because address(this).code.length will be nonzero. 

Also, because we no longer need to write to any state to track whether initialize() has been called, we can avoid the 20k storage gas cost. In fact, the cost for checking our own code size is only 2 gas, which means we have a 10,000x gas savings over the standard version. Pretty neat!


### [G-02] Storing the state variable by packing it

Packing and storing state variables consumes less gas. Changing the `depositFee` and `withdrawFee` state variables in the `collateral.sol` contract to uint128 causes one less slot to be used.

```solidity
1 result - 1 file

apps\smart-contracts\core\contracts\Collateral.sol:

  12:   address private manager;		       // slot 0
  13:   uint256 private depositFee;		       // slot 1
  14:   uint256 private withdrawFee;		       // slot 2
  15:   ICollateralHook private depositHook;	       // slot 3
  16:   ICollateralHook private withdrawHook;	       // slot 4	
  17:   ICollateralHook private managerWithdrawHook;   // slot 5
```
**Recommendation Code:**
```diff
apps\smart-contracts\core\contracts\Collateral.sol:

  12:   address private manager;		        // slot 0
- 13:   uint256 private depositFee;		        // slot 1
+ 13:   uint128 private depositFee;		        // slot 1
- 14:   uint256 private withdrawFee;		        // slot 2
+ 14:   uint128 private withdrawFee;		        // slot 1
  15:   ICollateralHook private depositHook;		// slot 2
  16:   ICollateralHook private withdrawHook;	        // slot 3	
  17:   ICollateralHook private managerWithdrawHook;	// slot 4
```
### [G-03] internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```solidity
2 results - 2 files

NFTScoreRequirement.sol:
  13:   function _satisfiesScoreRequirement(address account) internal view virtual returns (bool) {

PrePOMarketFactory.sol:
  41:   function _createPairTokens(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 _longTokenSalt, bytes32 _shortTokenSalt) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {

```

### [G-04] Using `delete` statement can save gas

```solidity
2 results - 2 files

DepositRecord.sol:
  37:     else { globalNetDepositAmount = 0; }

PrePOMarket.sol:
  99:     } else { _actualFee = 0; }
```

```diff
PrePOMarket.sol:
- 99:     } else { _actualFee = 0; }
+ 99:     } else { delete _actualFee; }
```

### [G-05] Using storage instead of memory for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct.


```solidity
7 results - 3 files

DepositHook.sol:
  73:   function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
  75:   function removeCollections(IERC721[] memory _collections) public override onlyRole(REMOVE_COLLECTIONS_ROLE) { super.removeCollections(_collections); }

DepositTradeHelper.sol:
  32:     ISwapRouter.ExactInputSingleParams memory exactInputSingleParams = ISwapRouter.ExactInputSingleParams(address(_collateral), tradeParams.tokenOut, POOL_FEE_TIER, msg.sender, tradeParams.deadline, _collateralAmountMinted, tradeParams.amountOutMinimum, tradeParams.sqrtPriceLimitX96);

NFTScoreRequirement.sol:
  22:   function setCollectionScores(IERC721[] memory collections, uint256[] memory scores) public virtual override {
  35:   function removeCollections(IERC721[] memory collections) public virtual override {
```

### [G-06] Use ``assembly`` to write _address storage values_ 

```solidity
23 results - 10 files

packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol:
  15   function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override {
  16:     _allowedMsgSenders = allowedMsgSenders;

packages/prepo-shared-contracts/contracts/AccountListCaller.sol:
  10   function setAccountList(IAccountList accountList) public virtual override {
  11:     _accountList = accountList;

apps/smart-contracts/core/contracts/Collateral.sol:
  29   constructor(IERC20 _newBaseToken, uint256 _newBaseTokenDecimals) {
  30:     baseToken = _newBaseToken;

  85   function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
  86:     manager = _newManager;

  102   function setDepositHook(ICollateralHook _newDepositHook) external override onlyRole(SET_DEPOSIT_HOOK_ROLE) {
  103:     depositHook = _newDepositHook;

  107   function setWithdrawHook(ICollateralHook _newWithdrawHook) external override onlyRole(SET_WITHDRAW_HOOK_ROLE) {
  108:     withdrawHook = _newWithdrawHook;

  112   function setManagerWithdrawHook(ICollateralHook _newManagerWithdrawHook) external override onlyRole(SET_MANAGER_WITHDRAW_HOOK_ROLE) {
  113:     managerWithdrawHook = _newManagerWithdrawHook;

apps/smart-contracts/core/contracts/DepositHook.sol:
  54   function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
  55:     collateral = _newCollateral;

  59   function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
  60:     depositRecord = _newDepositRecord;

apps/smart-contracts/core/contracts/DepositTradeHelper.sol:
  14   constructor(ICollateral collateral, ISwapRouter swapRouter) {
  15:     _collateral = collateral;
  16:     _baseToken = collateral.getBaseToken();

apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol:
  19   function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
  20:     collateral = _newCollateral;

  24   function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
  25:     depositRecord = _newDepositRecord;
  
apps/smart-contracts/core/contracts/WithdrawHook.sol:
  81   function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
  82:     collateral = _newCollateral;

  86   function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
  87:     depositRecord = _newDepositRecord;

apps/smart-contracts/core/contracts/PrePOMarket.sol:
  42   constructor(address _governance, address _collateral, ILongShortToken _longToken, ILongShortToken _shortToken,
  49:     collateral = IERC20(_collateral);
  50:     longToken = _longToken;
  51:     shortToken = _shortToken;

  109   function setMintHook(IMarketHook mintHook) external override onlyOwner {
  110:     _mintHook = mintHook;

  114   function setRedeemHook(IMarketHook redeemHook) external override onlyOwner {
  115:     _redeemHook = redeemHook;

apps/smart-contracts/core/contracts/TokenSender.sol:
  31   constructor(IERC20Metadata outputToken) {
  32:     _outputToken = outputToken;

packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol:
  11   function setTreasury(address treasury) public virtual override {
  12:     _treasury = treasury;

  20   function setTokenSender(ITokenSender tokenSender) public virtual override {
  21:     _tokenSender = tokenSender;
```
### [G-07] Setting the _constructor_ to `payable`

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of ```msg.value == 0``` and saves ```13 gas``` on deployment with no security risks.

```solidity
5 results - 5 files

apps/smart-contracts/core/contracts/Collateral.sol:
  29:   constructor(IERC20 _newBaseToken, uint256 _newBaseTokenDecimals) {

apps/smart-contracts/core/contracts/DepositRecord.sol:
  23:   constructor(uint256 _newGlobalNetDepositCap, uint256 _newUserDepositCap) {

apps/smart-contracts/core/contracts/DepositTradeHelper.sol:
  14:   constructor(ICollateral collateral, ISwapRouter swapRouter) {

apps/smart-contracts/core/contracts/PrePOMarket.sol:
  42:   constructor(address _governance, address _collateral, ILongShortToken _longToken, ILongShortToken _shortToken, uint256 _floorLongPayout, uint256 _ceilingLongPayout, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) {

apps/smart-contracts/core/contracts/TokenSender.sol:
  31:   constructor(IERC20Metadata outputToken) {

```

**Recommendation:**
Set the constructor to ```payable```

### [G-08]  Functions guaranteed to revert_ when callled by normal users can be marked `payable` 

If a function modifier or require such as onlyOwner-admin is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

```solidity
55 results - 11 files

apps/smart-contracts/core/contracts/Collateral.sol:
   80:   function managerWithdraw(uint256 _amount) external override onlyRole(MANAGER_WITHDRAW_ROLE) nonReentrant {
   85:   function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
   90:   function setDepositFee(uint256 _newDepositFee) external override onlyRole(SET_DEPOSIT_FEE_ROLE) {
   96:   function setWithdrawFee(uint256 _newWithdrawFee) external override onlyRole(SET_WITHDRAW_FEE_ROLE) {
  102:   function setDepositHook(ICollateralHook _newDepositHook) external override onlyRole(SET_DEPOSIT_HOOK_ROLE) {
  107:   function setWithdrawHook(ICollateralHook _newWithdrawHook) external override onlyRole(SET_WITHDRAW_HOOK_ROLE) {
  112:   function setManagerWithdrawHook(ICollateralHook _newManagerWithdrawHook) external override onlyRole(SET_MANAGER_WITHDRAW_HOOK_ROLE) {

apps/smart-contracts/core/contracts/DepositHook.sol:
  43:   function hook(address _sender, uint256 _amountBeforeFee, uint256 _amountAfterFee) external override onlyCollateral {
  54:   function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
  59:   function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
  64:   function setDepositsAllowed(bool _newDepositsAllowed) external override onlyRole(SET_DEPOSITS_ALLOWED_ROLE) {
  69:   function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) { super.setAccountList(accountList); }
  71:   function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) { super.setRequiredScore(_newRequiredScore); }
  73:   function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
  75:   function removeCollections(IERC721[] memory _collections) public override onlyRole(REMOVE_COLLECTIONS_ROLE) { super.removeCollections(_collections); }
  77:   function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) { super.setTreasury(_treasury); }
  79:   function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); }

apps/smart-contracts/core/contracts/DepositRecord.sol:
  28:   function recordDeposit(address _sender, uint256 _amount) external override onlyAllowedHooks {
  35:   function recordWithdrawal(uint256 _amount) external override onlyAllowedHooks {
  40:   function setGlobalNetDepositCap(uint256 _newGlobalNetDepositCap) external override onlyRole(SET_GLOBAL_NET_DEPOSIT_CAP_ROLE) {
  45:   function setUserDepositCap(uint256 _newUserDepositCap) external override onlyRole(SET_USER_DEPOSIT_CAP_ROLE) {
  50:   function setAllowedHook(address _hook, bool _allowed) external override onlyRole(SET_ALLOWED_HOOK_ROLE) {

apps/smart-contracts/core/contracts/LongShortToken.sol:
  11:   function mint(address _recipient, uint256 _amount) external onlyOwner { _mint(_recipient, _amount); }

apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol:
  19:   function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
  24:   function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
  29:   function setMinReservePercentage(uint256 _newMinReservePercentage) external override onlyRole(SET_MIN_RESERVE_PERCENTAGE_ROLE) {

apps/smart-contracts/core/contracts/MintHook.sol:
  16:   function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders { require(_accountList.isIncluded(sender), "minter not allowed"); }
  18:   function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); } 
  20:   function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }

apps/smart-contracts/core/contracts/PrePOMarket.sol:
  109:   function setMintHook(IMarketHook mintHook) external override onlyOwner {
  114:   function setRedeemHook(IMarketHook redeemHook) external override onlyOwner {
  119:   function setFinalLongPayout(uint256 _finalLongPayout) external override onlyOwner {
  126:   function setRedemptionFee(uint256 _redemptionFee) external override onlyOwner {

apps/smart-contracts/core/contracts/PrePOMarketFactory.sol:
  22:   function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
  36:   function setCollateralValidity(address _collateral, bool _validity) external override onlyOwner {

apps/smart-contracts/core/contracts/RedeemHook.sol:
  17:   function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders {
  26:   function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }  
  28:   function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); } 
  30:   function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); } 
  32:   function setTokenSender(ITokenSender tokenSender) public override onlyOwner { super.setTokenSender(tokenSender); }

apps/smart-contracts/core/contracts/TokenSender.sol:
  36:   function send(address recipient, uint256 unconvertedAmount) external override onlyAllowedMsgSenders {
  45:   function setPrice(IUintValue price) external override onlyRole(SET_PRICE_ROLE) {
  50:   function setPriceMultiplier(uint256 multiplier) external override onlyRole(SET_PRICE_MULTIPLIER_ROLE) {
  55:   function setScaledPriceLowerBound(uint256 lowerBound) external override onlyRole(SET_SCALED_PRICE_LOWER_BOUND_ROLE) {
  60:   function setAllowedMsgSenders(IAccountList allowedMsgSenders) public override onlyRole(SET_ALLOWED_MSG_SENDERS_ROLE) {

apps/smart-contracts/core/contracts/WithdrawHook.sol:
   57:   function hook(address _sender, uint256 _amountBeforeFee, uint256 _amountAfterFee) external override onlyCollateral {
   81:   function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
   86:   function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
   91:   function setWithdrawalsAllowed(bool _newWithdrawalsAllowed) external override onlyRole(SET_WITHDRAWALS_ALLOWED_ROLE) {
   96:   function setGlobalPeriodLength(uint256 _newGlobalPeriodLength) external override onlyRole(SET_GLOBAL_PERIOD_LENGTH_ROLE) {
  101:   function setUserPeriodLength(uint256 _newUserPeriodLength) external override onlyRole(SET_USER_PERIOD_LENGTH_ROLE) {
  106:   function setGlobalWithdrawLimitPerPeriod(uint256 _newGlobalWithdrawLimitPerPeriod) external override onlyRole(SET_GLOBAL_WITHDRAW_LIMIT_PER_PERIOD_ROLE) {
  111:   function setUserWithdrawLimitPerPeriod(uint256 _newUserWithdrawLimitPerPeriod) external override onlyRole(SET_USER_WITHDRAW_LIMIT_PER_PERIOD_ROLE) {
  116:   function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) {
  120:   function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) {
```

**Recommendation:**
Functions guaranteed to revert when called by normal users can be marked payable  (for only ```onlyOwner, onlyRole, onlyCollateral, onlyAllowedHooks or onlyAllowedMsgSenders``` functions)

### [G-09] State variables should be cached in stack variables rather than re-reading them from storage

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

```solidity
7 results 3 files

PrePOMarket.sol:
  66:     require(finalLongPayout > MAX_PAYOUT, "Market ended");
  67:     require(collateral.balanceOf(msg.sender) >= _amount, "Insufficient collateral");
  77:     require(longToken.balanceOf(msg.sender) >= _longAmount, "Insufficient long tokens");
  78:     require(shortToken.balanceOf(msg.sender) >= _shortAmount, "Insufficient short tokens");
  81:     if (finalLongPayout <= MAX_PAYOUT) {
 
PrePOMarketFactory.sol:
  23:     require(validCollateral[_collateral], "Invalid collateral");

WithdrawHook.sol:
  58:     require(withdrawalsAllowed, "withdrawals not allowed");
```
### [G-10] require() or revert() statements that check input arguments should be at the top of the function

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (2100 gas*) in a function that may ultimately revert in the unhappy case.

```solidity
1 result - 1 file

PrePOMarket.sol:
  43     require(_ceilingLongPayout > _floorLongPayout, "Ceiling must exceed floor");
  44     require(_expiryTime > block.timestamp, "Invalid expiry");
  45:     require(_ceilingLongPayout <= MAX_PAYOUT, "Ceiling cannot exceed 1");
```

**Recomemmendation Code:**
```diff

PrePOMarket.sol:
+ 45:     require(_ceilingLongPayout <= MAX_PAYOUT, "Ceiling cannot exceed 1");
   43     require(_ceilingLongPayout > _floorLongPayout, "Ceiling must exceed floor");
   44     require(_expiryTime > block.timestamp, "Invalid expiry");
- 45:     require(_ceilingLongPayout <= MAX_PAYOUT, "Ceiling cannot exceed 1");
```
### [G-11] Empty blocks should be removed or emit something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation. If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}}). Empty receive()/fallback() payable functions that are not used, can be removed to save deployment gas.

```solidity
1 result 1 file

LongShortToken.sol:
  9:   constructor(string memory name_, string memory symbol_) ERC20(name_, symbol_) {}

```
### [G-12] Optimize names to save gas
Contracts most called functions could simply save gas by function ordering via ```Method ID```. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because ```22 gas``` are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

**Context:** 
All Contracts


**Recommendation:** 
Find a lower ```method ID``` name for the most called functions for example Call() vs. Call1() is cheaper by ```22 gas```
For example, the function IDs in the ``` PrePOMarket.sol ``` contract will be the most used; A lower method ID may be given.

**Proof of Consept:**
https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

PrePOMarket.sol function names can be named and sorted according to METHOD ID

```js
Sighash   |   Function Signature
========================
a0712d68  =>  mint(uint256)
7cbc2373  =>  redeem(uint256,uint256)
6d296893  =>  setMintHook(IMarketHook)
26dee2d1  =>  setRedeemHook(IMarketHook)
bd30da0e  =>  setFinalLongPayout(uint256)
7dbc1df0  =>  setRedemptionFee(uint256)
acbaed0d  =>  getMintHook()
bad9cc73  =>  getRedeemHook()
5c1548fb  =>  getCollateral()
13c7df66  =>  getLongToken()
7a71e684  =>  getShortToken()
f12c7be6  =>  getFloorLongPayout()
78195e5d  =>  getCeilingLongPayout()
fa415632  =>  getFinalLongPayout()
2c6826f7  =>  getFloorValuation()
dab12f86  =>  getCeilingValuation()
9cd6f009  =>  getRedemptionFee()
25cb5bc0  =>  getExpiryTime()
2d9a37d3  =>  getMaxPayout()
f4e1fc41  =>  getFeeDenominator()
56a830d1  =>  getFeeLimit()
```
### [G-13] `x += y` costs more gas than `x = x + y` for state variables

```solidity
4 results - 2 files

apps/smart-contracts/core/contracts/DepositRecord.sol:
  31:     globalNetDepositAmount += _amount;
  32:     userToDeposits[_sender] += _amount;

apps/smart-contracts/core/contracts/WithdrawHook.sol:
  64:       globalAmountWithdrawnThisPeriod += _amountBeforeFee;
  71:       userToAmountWithdrawnThisPeriod[_sender] += _amountBeforeFee;

```

```diff
apps/smart-contracts/core/contracts/DepositRecord.sol#L31:

- globalNetDepositAmount += _amount;
+ globalNetDepositAmount = globalNetDepositAmount + _amount;

```

`x += y` costs more gas than `x = x  + y` for state variables.

### [G-14] It costs more gas to initialize non-constant/non-immutable variables to zero than to let the default of zero be applied

Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

***Current Code:***
```solidity
3 results - 1 file

NFTScoreRequirement.sol:
 
  25:     for (uint256 i = 0; i < numCollections; ) {
  37:     for (uint256 i = 0; i < numCollections; ) {
  58:     for (uint256 i = 0; i < numCollections; ) {
```

**Recommendation Code:**
```diff

- 25:     for (uint256 i = 0; i < numCollections; ) {
+ 25:     for (uint256 i ; i < numCollections; ) {
```

### [G-15] Comparison operators

In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `> and =`. Using strict comparison operators hence saves gas

```solidity
18 results - 7 files

apps/smart-contracts/core/contracts/Collateral.sol:
  91:     require(_newDepositFee <= FEE_LIMIT, "exceeds fee limit");
  97:     require(_newWithdrawFee <= FEE_LIMIT, "exceeds fee limit");
  
apps/smart-contracts/core/contracts/DepositRecord.sol:
  29:     require(_amount + globalNetDepositAmount <= globalNetDepositCap, "Global deposit cap exceeded");
  30:     require(_amount + userToDeposits[_sender] <= userDepositCap, "User deposit cap exceeded");
  
apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol:
  30:     require(_newMinReservePercentage <= PERCENT_DENOMINATOR, ">100%");

packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol:
  14:     return _requiredScore == 0 || getAccountScore(account) >= _requiredScore;

apps/smart-contracts/core/contracts/PrePOMarket.sol:
   44:    require(_expiryTime > block.timestamp, "Invalid expiry");
   45:    require(_ceilingLongPayout <= MAX_PAYOUT, "Ceiling cannot exceed 1");
   67:    require(collateral.balanceOf(msg.sender) >= _amount, "Insufficient collateral");
   77:    require(longToken.balanceOf(msg.sender) >= _longAmount, "Insufficient long tokens");
   78:    require(shortToken.balanceOf(msg.sender) >= _shortAmount, "Insufficient short tokens");
   81:    if (finalLongPayout <= MAX_PAYOUT) {
  120:    require(_finalLongPayout >= floorLongPayout, "Payout cannot be below floor");
  121:    require(_finalLongPayout <= ceilingLongPayout, "Payout cannot exceed ceiling");
  127:    require(_redemptionFee <= FEE_LIMIT, "Exceeds fee limit");
  
apps/smart-contracts/core/contracts/TokenSender.sol:
  38:     if (scaledPrice <= _scaledPriceLowerBound) return;
  
apps/smart-contracts/core/contracts/WithdrawHook.sol:
  63:     require(globalAmountWithdrawnThisPeriod + _amountBeforeFee <= globalWithdrawLimitPerPeriod, "global withdraw limit exceeded");
  70:     require(userToAmountWithdrawnThisPeriod[_sender] + _amountBeforeFee <= userWithdrawLimitPerPeriod, "user withdraw limit exceeded");
```

```diff
apps/smart-contracts/core/contracts/PrePOMarket.sol#L67

- 67:     require(collateral.balanceOf(msg.sender) >= _amount, "Insufficient collateral");
+ 67:     require(collateral.balanceOf(msg.sender) > _amount - 1, "Insufficient collateral");

```


**Recommendation:** 
Replace <= with <, and >= with >. Do not forget to increment/decrement the compared variable.

### [G-16] Use a more recent version of solidity

**Context:** 
All Contracts

Solidity 0.8.10 has a useful change that reduced gas costs of external calls which expect a return value. 

In 0.8.15 the conditions necessary for inlining are relaxed. Benchmarks show that the change significantly decreases the bytecode size (which impacts the deployment cost) while the effect on the runtime gas usage is smaller.

In 0.8.17 prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call; Simplify the starting offset of zero-length operations to zero. More efficient overflow checks for multiplication.

```solidity
apps/smart-contracts/core/contracts/PrePOMarket.sol:
  2: pragma solidity =0.8.7;
```
### [G-17] The solady Library's Ownable contract is significantly gas-optimized, which can be used

The project uses the ` onlyOwner ` authorization model I recommend using _Solady's highly gas optimized contract._

https://github.com/Vectorized/solady/blob/main/src/auth/OwnableRoles.sol

### [G-18] Use `Solmate SafeTransferLib ` contracts

**Description:**
Use the gas-optimized Solmate SafeTransferLib contract for Erc20

https://github.com/transmissions11/solmate/blob/main/src/utils/SafeTransferLib.sol

### [S-01] Use `v4.8.0 OpenZeppelin` contracts

**Description:**
The upcoming v4.8.0 version of OpenZeppelin provides many small gas optimizations.

https://github.com/OpenZeppelin/openzeppelin-contracts/releases/tag/v4.8.0-rc.0

```js
v4.8.0-rc.0
⛽ Many small optimizations
```
