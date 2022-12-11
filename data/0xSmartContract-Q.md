## Summary

### Low Risk Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
|[L-01]| Missing calls to `__ReentrancyGuard_init` functions of inherited contracts| 1 |
|[L-02]| Draft Openzeppelin Dependencies| 2 |
|[L-03]| There is a risk that the `proxyFee` variable is accidentally initialized to 0 and platform loses money| 4 |
|[L-04]| Use ```safeTransferOwnership``` instead of ```transferOwnership``` function| 2 |
|[L-05]| Owner can renounce Ownership| 3 |
|[L-06]| Missing Event for critical parameters init and change | 6 |
|[L-07]| A single point of failure  |  |
|[L-08]| Using vulnerable dependency of OpenZeppelin| 1 |
|[L-09]| Loss of precision due to rounding| 1 |
|[L-10]| initialize() function can be called by anybody| 2 |
|[L-11]| Use `Ownable2StepUpgradeable` instead of ` OwnableUpgradeable ` contract| 1 |

Total 11 issues

### Non-Critical Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
| [N-01]|Critical Address Changes Should Use Two-step Procedure|5|
| [N-02] |Initial value check is missing in Set Functions  |4|
| [N-03] |Use a single file for all system-wide constants| 43 |
| [N-04] |NatSpec comments should be increased in contracts | All Contracts |
| [N-05] |`Function writing` that does not comply with the `Solidity Style Guide` | All Conracts|
| [N-06] |Add a timelock to critical functions| 5 |
| [N-07] |Use a more recent version of Solidity| All Conracts |
| [N-08] |Solidity compiler optimizations can be problematic |  |
| [N-09] |For modern and more readable code; update import usages| 63 |
| [N-10] |Include return parameters in NatSpec comments| All Conracts  |
| [N-11] |Omissions in Events| 11 |
| [N-12] |Long lines are not suitable for the ‘Solidity Style Guide’ | 6 |
| [N-13] |Avoid _shadowing_ `inherited state variables`| 1 |
| [N-14] |Open TODOs| 1 |
| [N-15] |Constant values such as a call to keccak256(), should used to immutable rather than constant | 35 |
| [N-16] |Mark visibility of initialize(...) functions as ``external``| 2 |

Total 16 issues



### Suggestions
| Number | Suggestion Details |
|:--:|:-------|
|[S-01]|Make the Test Context with Solidity |
|[S-02]|Project Upgrade and Stop Scenario should be |
|[S-03]|Generate perfect code headers every time |

Total 3 suggestions


### [L-01] Missing calls to `__ReentrancyGuard_init` functions of inherited contracts

Most contracts use the delegateCall proxy pattern and hence their implementations require the use of initialize() functions instead of constructors. This requires derived contracts to call the corresponding init functions of their inherited base contracts. This is done in most places except a few.

Impact: The inherited base classes do not get initialized which may lead to undefined behavior.


Missing call to __ReentrancyGuard_init:
[Collateral.sol#L35-L37](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L35-L37)

[PrePOMarketFactory.sol#L16](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L16)

Recommended Mitigation Steps
Add missing calls to init functions of inherited contracts.

```diff

 function initialize(string memory _name, string memory _symbol) public initializer {
    __SafeAccessControlEnumerable_init();
    __ERC20_init(_name, _symbol);
    __ERC20Permit_init(_name);
+   __ReentrancyGuard_init();
  }
```


### [L-02] Draft Openzeppelin Dependencies

The `Collateral.sol` and `DepositTradeHelper.sol` contracts utilised `draft-IERC20Permit.sol` and `draftERC20PermitUpgradeable.sol`, an OpenZeppelin contracts. This contracts are still a draft and are not considered ready for mainnet use. OpenZeppelin contracts may be considered draft contracts if they have not received adequate security auditing or are liable to change with future development.


```solidity
2 results - 2 files

/Collateral.sol:
5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-ERC20PermitUpgradeable.sol";

/DepositTradeHelper.sol:
  6: import "@openzeppelin/contracts/token/ERC20/extensions/draft-IERC20Permit.sol";

```


### [L-03] There is a risk that the `proxyFee` variable is accidentally initialized to 0 and platform loses money


### Impact

There is a risk that the `fees` variables are accidentally initialized to 0

Starting `setWithdrawFee` with 0 is an administrative decision, but since there is no information about this in the documentation and NatSpec comments during the audit, we can assume that it will not be 0 also other  fee setter functions

In addition, it is a strong belief that it will not be 0, as it is an issue that will affect the platform revenues.

Although the value initialized with 0 by mistake or forgetting can be changed later by onlyOwner/onlyRole, in the first place it can be exploited by users and cause huge amount  usage

`require == 0` should not be made because of the possibility of the platform defining the `0` rate.


```js
4 files

/Collateral.sol:
   90:   function setDepositFee(uint256 _newDepositFee) external override onlyRole(SET_DEPOSIT_FEE_ROLE) {
   91      require(_newDepositFee <= FEE_LIMIT, "exceeds fee limit");

   96:   function setWithdrawFee(uint256 _newWithdrawFee) external override onlyRole(SET_WITHDRAW_FEE_ROLE) {
   97      require(_newWithdrawFee <= FEE_LIMIT, "exceeds fee limit");
 
  126:   function setRedemptionFee(uint256 _redemptionFee) external override onlyOwner {
  127      require(_redemptionFee <= FEE_LIMIT, "Exceeds fee limit");

/TokenSender.sol:
  45:   function setPrice(IUintValue price) external override onlyRole(SET_PRICE_ROLE) {
  46      _price = price;

```


### [L-04] Use ```safeTransferOwnership``` instead of ```transferOwnership``` function

**Context:**
```solidity
2 results - 2 files

/LongShortToken.sol:
  8: contract LongShortToken is ERC20Burnable, Ownable {

/PrePOMarket.sol:
  10: contract PrePOMarket is IPrePOMarket, Ownable, ReentrancyGuard {

```

**Description:**
```transferOwnership``` function is used to change Ownership from ```Ownable.sol```.

Use a 2 structure transferOwnership which is safer. 
```safeTransferOwnership```,  use it is more secure due to 2-stage ownership transfer.

**Recommendation:**
Use ``Ownable2Step.sol``
[Ownable2Step.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol)


### [L-05] Owner can renounce Ownership

**Context:**
```solidity
6 results - 6 files

/DepositTradeHelper.sol:
  5: import "prepo-shared-contracts/contracts/SafeOwnable.sol";
  8: contract DepositTradeHelper is IDepositTradeHelper, SafeOwnable {

/LongShortToken.sol:
  5: import "@openzeppelin/contracts/access/Ownable.sol";
  8: contract LongShortToken is ERC20Burnable, Ownable {

/MintHook.sol:
   8: import "prepo-shared-contracts/contracts/SafeOwnable.sol";
  10: contract MintHook is IMarketHook, AllowedMsgSenders, AccountListCaller, SafeOwnable {

/PrePOMarket.sol:
   7: import "@openzeppelin/contracts/access/Ownable.sol";
  10: contract PrePOMarket is IPrePOMarket, Ownable, ReentrancyGuard {

/PrePOMarketFactory.sol:
   8: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
  12: contract PrePOMarketFactory is IPrePOMarketFactory, OwnableUpgradeable, ReentrancyGuardUpgradeable {

/RedeemHook.sol:
   9: import "prepo-shared-contracts/contracts/SafeOwnable.sol";
  11: contract RedeemHook is IMarketHook, AllowedMsgSenders, AccountListCaller, TokenSenderCaller, SafeOwnable {

```

**Description:**
Typically, the contract’s owner is the account that deploys the contract. As a result, the owner is able to perform certain privileged activities.

The Openzeppelin’s Ownable used in this project contract implements renounceOwnership. This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

`onlyOwner` functions;
```js

13 results - 5 files

/LongShortToken.sol:
  10  
  11:   function mint(address _recipient, uint256 _amount) external onlyOwner { _mint(_recipient, _amount); }
  12  }

/MintHook.sol:
  17  
  18:   function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
  19  
  20:   function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
  21  }

/PrePOMarket.sol:
  108  
  109:   function setMintHook(IMarketHook mintHook) external override onlyOwner {
  110      _mintHook = mintHook;

  113  
  114:   function setRedeemHook(IMarketHook redeemHook) external override onlyOwner {
  115      _redeemHook = redeemHook;

  118  
  119:   function setFinalLongPayout(uint256 _finalLongPayout) external override onlyOwner {
  120      require(_finalLongPayout >= floorLongPayout, "Payout cannot be below floor");

  125  
  126:   function setRedemptionFee(uint256 _redemptionFee) external override onlyOwner {
  127      require(_redemptionFee <= FEE_LIMIT, "Exceeds fee limit");

/PrePOMarketFactory.sol:
  21  
  22:   function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
  23      require(validCollateral[_collateral], "Invalid collateral");

  35  
  36:   function setCollateralValidity(address _collateral, bool _validity) external override onlyOwner {
  37      validCollateral[_collateral] = _validity;

/RedeemHook.sol:
  25  
  26:   function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
  27  
  28:   function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
  29  
  30:   function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
  31  
  32:   function setTokenSender(ITokenSender tokenSender) public override onlyOwner { super.setTokenSender(tokenSender); }
  33  }

```

**Recommendation:**
We recommend to either reimplement the function to disable it or to clearly specify if it is part of the contract design.

### [L-06] Missing Event for critical parameters init and change

**Context:**
```js

6 results - 6 files

/Collateral.sol:
  28  
  29:   constructor(IERC20 _newBaseToken, uint256 _newBaseTokenDecimals) {
  30      baseToken = _newBaseToken;

/DepositRecord.sol:
  22  
  23:   constructor(uint256 _newGlobalNetDepositCap, uint256 _newUserDepositCap) {
  24      globalNetDepositCap = _newGlobalNetDepositCap;

/DepositTradeHelper.sol:
  13  
  14:   constructor(ICollateral collateral, ISwapRouter swapRouter) {
  15      _collateral = collateral;

/LongShortToken.sol:
  8  contract LongShortToken is ERC20Burnable, Ownable {
  9:   constructor(string memory name_, string memory symbol_) ERC20(name_, symbol_) {}
  10  

/PrePOMarket.sol:
  41     */
  42:   constructor(address _governance, address _collateral, ILongShortToken _longToken, ILongShortToken _shortToken, uint256 _floorLongPayout, uint256 _ceilingLongPayout, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) {
  43      require(_ceilingLongPayout > _floorLongPayout, "Ceiling must exceed floor");

/TokenSender.sol:
  30  
  31:   constructor(IERC20Metadata outputToken) {
  32      _outputToken = outputToken;

```

**Description:**
Events help non-contract tools to track changes, and events prevent users from being surprised by changes

**Recommendation:**
Add Event-Emit

### [L-07]  A single point of failure 

```solidity
6 results - 6 files

/DepositTradeHelper.sol:
  5: import "prepo-shared-contracts/contracts/SafeOwnable.sol";
  8: contract DepositTradeHelper is IDepositTradeHelper, SafeOwnable {

/LongShortToken.sol:
  5: import "@openzeppelin/contracts/access/Ownable.sol";
  8: contract LongShortToken is ERC20Burnable, Ownable {

/MintHook.sol:
   8: import "prepo-shared-contracts/contracts/SafeOwnable.sol";
  10: contract MintHook is IMarketHook, AllowedMsgSenders, AccountListCaller, SafeOwnable {

/PrePOMarket.sol:
   7: import "@openzeppelin/contracts/access/Ownable.sol";
  10: contract PrePOMarket is IPrePOMarket, Ownable, ReentrancyGuard {

/PrePOMarketFactory.sol:
   8: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
  12: contract PrePOMarketFactory is IPrePOMarketFactory, OwnableUpgradeable, ReentrancyGuardUpgradeable {

/RedeemHook.sol:
   9: import "prepo-shared-contracts/contracts/SafeOwnable.sol";
  11: contract RedeemHook is IMarketHook, AllowedMsgSenders, AccountListCaller, TokenSenderCaller, SafeOwnable {

```


## Impact

The `owner` role has a single point of failure and `onlyOwner` can use critical a few functions.

`owner` role in the project:
Owner is not behind a multisig and changes are not behind a timelock.

Even if protocol admins/developers are not malicious there is still a chance for Owner keys to be stolen. In such a case, the attacker can cause serious damage to the project due to important functions. In such a case, users who have invested in project will suffer high financial losses.


`onlyOwner` functions;
```js

13 results - 5 files

/LongShortToken.sol:
  10  
  11:   function mint(address _recipient, uint256 _amount) external onlyOwner { _mint(_recipient, _amount); }
  12  }

/MintHook.sol:
  17  
  18:   function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
  19  
  20:   function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
  21  }

/PrePOMarket.sol:
  108  
  109:   function setMintHook(IMarketHook mintHook) external override onlyOwner {
  110      _mintHook = mintHook;

  113  
  114:   function setRedeemHook(IMarketHook redeemHook) external override onlyOwner {
  115      _redeemHook = redeemHook;

  118  
  119:   function setFinalLongPayout(uint256 _finalLongPayout) external override onlyOwner {
  120      require(_finalLongPayout >= floorLongPayout, "Payout cannot be below floor");

  125  
  126:   function setRedemptionFee(uint256 _redemptionFee) external override onlyOwner {
  127      require(_redemptionFee <= FEE_LIMIT, "Exceeds fee limit");

/PrePOMarketFactory.sol:
  21  
  22:   function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
  23      require(validCollateral[_collateral], "Invalid collateral");

  35  
  36:   function setCollateralValidity(address _collateral, bool _validity) external override onlyOwner {
  37      validCollateral[_collateral] = _validity;

/RedeemHook.sol:
  25  
  26:   function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
  27  
  28:   function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
  29  
  30:   function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
  31  
  32:   function setTokenSender(ITokenSender tokenSender) public override onlyOwner { super.setTokenSender(tokenSender); }
  33  }

```

This increases the risk of ```A single point of failure```




## Tools Used
Manuel Code Review


## Recommended Mitigation Steps
Add a time lock to  critical functions. Admin-only functions that change critical parameters should emit events and have timelocks. 

Events allow capturing the changed parameters so that off-chain tools/interfaces can register such changes with timelocks that allow users to evaluate them and consider if they would like to engage/exit based on how they perceive the changes as affecting the trustworthiness of the protocol or profitability of the implemented financial services.

Allow only multi-signature wallets to call the function to reduce the likelihood of an attack.

https://twitter.com/danielvf/status/1572963475101556738?s=20&t=V1kvzfJlsx-D2hfnG0OmuQ

Also detail them in documentation and NatSpec comments


### [L-08] Using vulnerable dependency of OpenZeppelin

The package.json configuration file says that the project is using 4.7.3 of OZ which has a not last update version

```js
1 result - 1 file

packages/prepo-shared-contracts/package.json:
  26    },
  27:   "dependencies": {
  28:     "@openzeppelin/contracts": "4.7.3",
  29:     "@openzeppelin/contracts-upgradeable": "4.7.3",

```

**Recommendation:**
Use patched versions
Latest non vulnerable version 4.8.0


### [L-09] Loss of precision due to rounding


```solidity

/Collateral.sol:
45:   function deposit(address _recipient, uint256 _amount) external override nonReentrant returns (uint256) {
  46:     uint256 _fee = (_amount * depositFee) / FEE_DENOMINATOR;
```

### [L-10] initialize() function can be called by anybody

`initialize()` function can be called anybody when the contract is not initialized.

More importantly, if someone else runs this function, they will have full authority because of the `__Ownable_init()` function.

Here is a definition of `initialize()` function.

```solidity

2 results - 2 files

/Collateral.sol:
  33  
  34:   function initialize(string memory _name, string memory _symbol) public initializer {
  35      __SafeAccessControlEnumerable_init();

/PrePOMarketFactory.sol:
  15  
  16:   function initialize() public initializer { OwnableUpgradeable.__Ownable_init(); }
  17 

```


### Recommended Mitigation Steps

Add a control that makes `initialize()` only call the Deployer Contract or EOA;

```js
if (msg.sender != DEPLOYER_ADDRESS) {
            revert NotDeployer();
        }
```

### [L-11] Use `Ownable2StepUpgradeable` instead of ` OwnableUpgradeable ` contract

**Context:**
```solidity
/PrePOMarketFactory.sol:
   8: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

  12: contract PrePOMarketFactory is IPrePOMarketFactory, OwnableUpgradeable, ReentrancyGuardUpgradeable {

  16:   function initialize() public initializer { OwnableUpgradeable.__Ownable_init(); }

```
**Description:**
```transferOwnership``` function is used to change Ownership from ```OwnableUpgradeable.sol```.

There is another Openzeppelin Ownable contract (Ownable2StepUpgradeable.sol) has  ` transferOwnership` function ,  use it is more secure due to 2-stage ownership transfer.

[Ownable2StepUpgradeable.sol](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/master/contracts/access/Ownable2StepUpgradeable.sol)

### [N-01] Critical Address Changes Should Use Two-step Procedure

The critical procedures should be two step process.


```solidity
5 results

/Collateral.sol:
   84  
   85:   function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
   86      manager = _newManager;

/ManagerWithdrawHook.sol:
  18  
  19:   function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
  20      collateral = _newCollateral;

  24:   function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
  25      depositRecord = _newDepositRecord;

/TokenSenderCaller.sol:
  11:   function setTreasury(address treasury) public virtual override {
  12      _treasury = treasury;


/WithdrawHook.sol:
  116:   function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) {
  117      super.setTreasury(_treasury);
```

Recommended Mitigation Steps
Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.


### [N-02] Initial value check is missing in Set Functions

**Context:**


```js
4 results

/Collateral.sol:
   90:   function setDepositFee(uint256 _newDepositFee) external override onlyRole(SET_DEPOSIT_FEE_ROLE) {
   91      require(_newDepositFee <= FEE_LIMIT, "exceeds fee limit");

   96:   function setWithdrawFee(uint256 _newWithdrawFee) external override onlyRole(SET_WITHDRAW_FEE_ROLE) {
   97      require(_newWithdrawFee <= FEE_LIMIT, "exceeds fee limit");
 
  126:   function setRedemptionFee(uint256 _redemptionFee) external override onlyOwner {
  127      require(_redemptionFee <= FEE_LIMIT, "Exceeds fee limit");

/TokenSender.sol:
  45:   function setPrice(IUintValue price) external override onlyRole(SET_PRICE_ROLE) {
  46      _price = price;

```



Checking whether the current value and the new value are the same should be added

### [N-03] Use a single file for all system-wide constants

There are many addresses and constants used in the system. It is recommended to put the most used ones in one file (for example constants.sol, use inheritance to access these values)

  This will help with readability and easier maintenance for future changes. 

constants.sol
Use and import this file in contracts that require access to these values. This is just a suggestion, in some use cases this may result in higher gas usage in the distribution

```solidity

43 results - 8 files

/Collateral.sol:
  18  
  19:   uint256 public constant FEE_DENOMINATOR = 1000000;
  20:   uint256 public constant FEE_LIMIT = 100000;
  21:   bytes32 public constant MANAGER_WITHDRAW_ROLE = keccak256("Collateral_managerWithdraw(uint256)");
  22:   bytes32 public constant SET_MANAGER_ROLE = keccak256("Collateral_setManager(address)");
  23:   bytes32 public constant SET_DEPOSIT_FEE_ROLE = keccak256("Collateral_setDepositFee(uint256)");
  24:   bytes32 public constant SET_WITHDRAW_FEE_ROLE = keccak256("Collateral_setWithdrawFee(uint256)");
  25:   bytes32 public constant SET_DEPOSIT_HOOK_ROLE = keccak256("Collateral_setDepositHook(ICollateralHook)");
  26:   bytes32 public constant SET_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setWithdrawHook(ICollateralHook)");
  27:   bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");
  28  

/DepositHook.sol:
  16  
  17:   bytes32 public constant SET_COLLATERAL_ROLE = keccak256("DepositHook_setCollateral(address)");
  18:   bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("DepositHook_setDepositRecord(address)");
  19:   bytes32 public constant SET_DEPOSITS_ALLOWED_ROLE = keccak256("DepositHook_setDepositsAllowed(bool)");
  20:   bytes32 public constant SET_ACCOUNT_LIST_ROLE = keccak256("DepositHook_setAccountList(IAccountList)");
  21:   bytes32 public constant SET_REQUIRED_SCORE_ROLE = keccak256("DepositHook_setRequiredScore(uint256)");
  22:   bytes32 public constant SET_COLLECTION_SCORES_ROLE = keccak256("DepositHook_setCollectionScores(IERC721[],uint256[])");
  23:   bytes32 public constant REMOVE_COLLECTIONS_ROLE = keccak256("DepositHook_removeCollections(IERC721[])");
  24:   bytes32 public constant SET_TREASURY_ROLE = keccak256("DepositHook_setTreasury(address)");
  25:   bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("DepositHook_setTokenSender(ITokenSender)");
  26  

/DepositRecord.sol:
  13  
  14:   bytes32 public constant SET_GLOBAL_NET_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setGlobalNetDepositCap(uint256)");
  15:   bytes32 public constant SET_USER_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setUserDepositCap(uint256)");
  16:   bytes32 public constant SET_ALLOWED_HOOK_ROLE = keccak256("DepositRecord_setAllowedHook(address)");
  17  

/DepositTradeHelper.sol:
  11    ISwapRouter private immutable _swapRouter;
  12:   uint24 public constant override POOL_FEE_TIER = 10000;
  13  

/ManagerWithdrawHook.sol:
  11  
  12:   uint256 public constant PERCENT_DENOMINATOR = 1000000;
  13:   bytes32 public constant SET_COLLATERAL_ROLE = keccak256("ManagerWithdrawHook_setCollateral(address)");
  14:   bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("ManagerWithdrawHook_setDepositRecord(address)");
  15:   bytes32 public constant SET_MIN_RESERVE_PERCENTAGE_ROLE = keccak256("ManagerWithdrawHook_setMinReservePercentage(uint256)");
  16  

/PrePOMarket.sol:
  28  
  29:   uint256 private constant MAX_PAYOUT = 1e18;
  30:   uint256 private constant FEE_DENOMINATOR = 1000000;
  31:   uint256 private constant FEE_LIMIT = 100000;
  32  

/TokenSender.sol:
  24  
  25:   uint256 public constant MULTIPLIER_DENOMINATOR = 10000;
  26:   bytes32 public constant SET_PRICE_ROLE = keccak256("TokenSender_setPrice(IUintValue)");
  27:   bytes32 public constant SET_PRICE_MULTIPLIER_ROLE = keccak256("TokenSender_setPriceMultiplier(uint256)");
  28:   bytes32 public constant SET_SCALED_PRICE_LOWER_BOUND_ROLE = keccak256("TokenSender_setScaledPriceLowerBound(uint256)");
  29:   bytes32 public constant SET_ALLOWED_MSG_SENDERS_ROLE = keccak256("TokenSender_setAllowedMsgSenders(IAccountList)");
  30  

/WithdrawHook.sol:
  21  
  22:   bytes32 public constant SET_COLLATERAL_ROLE = keccak256("WithdrawHook_setCollateral(address)");
  23:   bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("WithdrawHook_setDepositRecord(address)");
  24:   bytes32 public constant SET_WITHDRAWALS_ALLOWED_ROLE = keccak256("WithdrawHook_setWithdrawalsAllowed(bool)");
  25:   bytes32 public constant SET_GLOBAL_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setGlobalPeriodLength(uint256)");
  26:   bytes32 public constant SET_USER_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setUserPeriodLength(uint256)");
  27:   bytes32 public constant SET_GLOBAL_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setGlobalWithdrawLimitPerPeriod(uint256)");
  28:   bytes32 public constant SET_USER_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setUserWithdrawLimitPerPeriod(uint256)");
  29:   bytes32 public constant SET_TREASURY_ROLE = keccak256("WithdrawHook_setTreasury(address)");
  30:   bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("WithdrawHook_setTokenSender(ITokenSender)");
  31 
```

### [N-04] NatSpec comments should be increased in contracts

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.
In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.
https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

**Recommendation:**
NatSpec comments should be increased in contracts

### [N-05] `Function writing` that does not comply with the `Solidity Style Guide`

**Context:**
All Contracts

**Description:**
Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

 constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private
within a grouping, place the view and pure functions last

### [N-06] Add a timelock to critical functions

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user).
Consider adding a timelock to:

```solidity

5 results 

/Collateral.sol:
   84  
   85:   function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
   86      manager = _newManager;

/ManagerWithdrawHook.sol:
  18  
  19:   function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
  20      collateral = _newCollateral;

  23  
  24:   function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
  25      depositRecord = _newDepositRecord;

/TokenSenderCaller.sol:
  11:   function setTreasury(address treasury) public virtual override {
  12      _treasury = treasury;


/WithdrawHook.sol:
  116:   function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) {
  117      super.setTreasury(_treasury);

```


### [N-07] Use a more recent version of Solidity

**Context:**
All contracts

**Description:**
For security, it is best practice to use the latest Solidity version.
For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md


**Recommendation:**
Old version of Solidity is used , newer version can be used `(0.8.17)` 

### [N-08] Solidity compiler optimizations can be problematic

```js
packages/prepo-shared-contracts/hardhat.config.ts:
  41  
  42: const config: HardhatUserConfig = {
  43:   ...hardhatConfig,
  44:   solidity: {
  45:     compilers: [
  46:       {
  47:         version: '0.8.7',
  48:         settings: {
  49:           optimizer: {
  50:             enabled: true,
  51:             runs: 99999,
  52:           },
  53          },


```

**Description:**
Protocol has enabled optional compiler optimizations in Solidity.
There have been several optimization bugs with security implications. Moreover, optimizations are actively being developed. Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. 

Therefore, it is unclear how well they are being tested and exercised.
High-severity security issues due to optimization bugs have occurred in the past. A high-severity bug in the emscripten-generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. 

Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. More recently, another bug due to the incorrect caching of keccak256 was reported.
A compiler audit of Solidity from November 2018 concluded that the optional optimizations may not be safe.
It is likely that there are latent bugs related to optimization and that new bugs will be introduced due to future optimizations.

Exploit Scenario
A latent or future bug in Solidity compiler optimizations—or in the Emscripten transpilation to solc-js—causes a security vulnerability in the contracts.


**Recommendation:**
Short term, measure the gas savings from optimizations and carefully weigh them against the possibility of an optimization-related bug.
Long term, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.


### [N-09] For modern and more readable code; update import usages

**Context:**

```solidity
63 results - 16 files
```


**Description:**
Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct `polluted the source code` with an unnecessary object we were not using because we did not need it. 
This was breaking the rule of modularity and modular programming: `only import what you need` Specific imports with curly braces allow us to apply this rule better.

**Recommendation:**
`import {contract1 , contract2} from "filename.sol";`

A good example from the ArtGobblers project;
```js
import {Owned} from "solmate/auth/Owned.sol";
import {ERC721} from "solmate/tokens/ERC721.sol";
import {LibString} from "solmate/utils/LibString.sol";
import {MerkleProofLib} from "solmate/utils/MerkleProofLib.sol";
import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
import {ERC1155, ERC1155TokenReceiver} from "solmate/tokens/ERC1155.sol";
import {toWadUnsafe, toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";
```

### [N-10] Include return parameters in NatSpec comments

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

**Recommendation:**
Include return parameters in NatSpec comments

_Recommendation  Code Style: (from Uniswap3)_
```js
    /// @notice Adds liquidity for the given recipient/tickLower/tickUpper position
    /// @dev The caller of this method receives a callback in the form of IUniswapV3MintCallback#uniswapV3MintCallback
    /// in which they must pay any token0 or token1 owed for the liquidity. The amount of token0/token1 due depends
    /// on tickLower, tickUpper, the amount of liquidity, and the current price.
    /// @param recipient The address for which the liquidity will be created
    /// @param tickLower The lower tick of the position in which to add liquidity
    /// @param tickUpper The upper tick of the position in which to add liquidity
    /// @param amount The amount of liquidity to mint
    /// @param data Any data that should be passed through to the callback
    /// @return amount0 The amount of token0 that was paid to mint the given amount of liquidity. Matches the value in the callback
    /// @return amount1 The amount of token1 that was paid to mint the given amount of liquidity. Matches the value in the callback
    function mint(
        address recipient,
        int24 tickLower,
        int24 tickUpper,
        uint128 amount,
        bytes calldata data
    ) external returns (uint256 amount0, uint256 amount1);
```

### [N-11] Omissions in Events

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters

The events should include the new value and old value where possible:

```solidity

11 results

/AccountListCaller.sol:
   9  
  10:   function setAccountList(IAccountList accountList) public virtual override {
  11:     _accountList = accountList;
  12:     emit AccountListChange(accountList);
  13:   }

/AllowedMsgSenders.sol:
  14  
  15:   function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override {
  16:     _allowedMsgSenders = allowedMsgSenders;
  17:     emit AllowedMsgSendersChange(allowedMsgSenders);
  18:   }

/Collateral.sol:
   84  
   85:   function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
   86:     manager = _newManager;
   87:     emit ManagerChange(_newManager);
   88:   }
   89: 
   90:   function setDepositFee(uint256 _newDepositFee) external override onlyRole(SET_DEPOSIT_FEE_ROLE) {
   91:     require(_newDepositFee <= FEE_LIMIT, "exceeds fee limit");
   92:     depositFee = _newDepositFee;
   93:     emit DepositFeeChange(_newDepositFee);
   94:   }
   95: 
   96:   function setWithdrawFee(uint256 _newWithdrawFee) external override onlyRole(SET_WITHDRAW_FEE_ROLE) {
   97:     require(_newWithdrawFee <= FEE_LIMIT, "exceeds fee limit");
   98:     withdrawFee = _newWithdrawFee;
   99:     emit WithdrawFeeChange(_newWithdrawFee);
  100:   }
  101: 
  102:   function setDepositHook(ICollateralHook _newDepositHook) external override onlyRole(SET_DEPOSIT_HOOK_ROLE) {
  103:     depositHook = _newDepositHook;
  104:     emit DepositHookChange(address(_newDepositHook));
  105:   }
  106: 
  107:   function setWithdrawHook(ICollateralHook _newWithdrawHook) external override onlyRole(SET_WITHDRAW_HOOK_ROLE) {
  108:     withdrawHook = _newWithdrawHook;
  109:     emit WithdrawHookChange(address(_newWithdrawHook));
  110:   }
  111: 
  112:   function setManagerWithdrawHook(ICollateralHook _newManagerWithdrawHook) external override onlyRole(SET_MANAGER_WITHDRAW_HOOK_ROLE) {
  113:     managerWithdrawHook = _newManagerWithdrawHook;
  114:     emit ManagerWithdrawHookChange(address(_newManagerWithdrawHook));
  115:   }


/DepositHook.sol:
  53  
  54:   function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
  55:     collateral = _newCollateral;
  56:     emit CollateralChange(address(_newCollateral));
  57:   }
  58: 
  59:   function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
  60:     depositRecord = _newDepositRecord;
  61:     emit DepositRecordChange(address(_newDepositRecord));
  62:   }
  63: 
  64:   function setDepositsAllowed(bool _newDepositsAllowed) external override onlyRole(SET_DEPOSITS_ALLOWED_ROLE) {
  65:     depositsAllowed = _newDepositsAllowed;
  66:     emit DepositsAllowedChange(_newDepositsAllowed);
  67:   }


```

## [N-12] Long lines are not suitable for the ‘Solidity Style Guide’

**Context:**
[Collateral.sol#L9](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L9)
[Collateral.sol#L112](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L112)
[DepositHook.sol#L69-L79](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L69-L79)
[DepositRecord.sol#L40](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L40)
[DepositTradeHelper.sol#L22-L32](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L22-L32)
[RedeemHook.sol#L26](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L26)

**Description:**
It is generally recommended that lines in the source code should not exceed 80-120 characters. Today's screens are much larger, so in some cases it makes sense to expand that. The lines above should be split when they reach that length, as the files will most likely be on GitHub and GitHub always uses a scrollbar when the length is more than 164 characters.

(why-is-80-characters-the-standard-limit-for-code-width)[https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width]

**Recommendation:**
Multiline output parameters and return statements should follow the same style recommended for wrapping long lines found in the Maximum Line Length section.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html#introduction

```js
thisFunctionCallIsReallyLong(
    longArgument1,
    longArgument2,
    longArgument3
);
```
## [N-13] Avoid _shadowing_ inherited state variables

**Context:**
```solidity
/DepositHook.sol:
79:   function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); }

/TokenSenderCaller.sol:
  7: contract TokenSenderCaller is ITokenSenderCaller {
  9:   ITokenSender internal _tokenSender;


```
`_tokenSender` is shadowed

**Recommendation:**
Avoid using variables with the same name, including inherited in the same contract, if used, it must be specified in the NatSpec comments.


### [N-14] Open TODOs

**Context:**
```solidity
/TokenSender.sol:
  14    AllowedMsgSenders,
  15:   WithdrawERC20, // TODO: Access control when WithdrawERC20 updated

```

**Recommendation:**
Use temporary TODOs as you work on a feature, but make sure to treat them before merging. Either add a link to a proper issue in your TODO, or remove it from the code.

### [N-15] Constant values such as a call to keccak256(), should used to immutable rather than constant

There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts.

While it doesn't save any gas because the compiler knows that developers often make this mistake, it's still best to use the right tool for the task at hand.

Constants should be used for literal values written into the code, and immutable variables should be used for expressions, or values calculated in, or passed into the constructor.


```js
35 results - 6 files

/Collateral.sol:
  21:   bytes32 public constant MANAGER_WITHDRAW_ROLE = keccak256("Collateral_managerWithdraw(uint256)");
  22:   bytes32 public constant SET_MANAGER_ROLE = keccak256("Collateral_setManager(address)");
  23:   bytes32 public constant SET_DEPOSIT_FEE_ROLE = keccak256("Collateral_setDepositFee(uint256)");
  24:   bytes32 public constant SET_WITHDRAW_FEE_ROLE = keccak256("Collateral_setWithdrawFee(uint256)");
  25:   bytes32 public constant SET_DEPOSIT_HOOK_ROLE = keccak256("Collateral_setDepositHook(ICollateralHook)");
  26:   bytes32 public constant SET_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setWithdrawHook(ICollateralHook)");
  27:   bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");

/DepositHook.sol:
  17:   bytes32 public constant SET_COLLATERAL_ROLE = keccak256("DepositHook_setCollateral(address)");
  18:   bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("DepositHook_setDepositRecord(address)");
  19:   bytes32 public constant SET_DEPOSITS_ALLOWED_ROLE = keccak256("DepositHook_setDepositsAllowed(bool)");
  20:   bytes32 public constant SET_ACCOUNT_LIST_ROLE = keccak256("DepositHook_setAccountList(IAccountList)");
  21:   bytes32 public constant SET_REQUIRED_SCORE_ROLE = keccak256("DepositHook_setRequiredScore(uint256)");
  22:   bytes32 public constant SET_COLLECTION_SCORES_ROLE = keccak256("DepositHook_setCollectionScores(IERC721[],uint256[])");
  23:   bytes32 public constant REMOVE_COLLECTIONS_ROLE = keccak256("DepositHook_removeCollections(IERC721[])");
  24:   bytes32 public constant SET_TREASURY_ROLE = keccak256("DepositHook_setTreasury(address)");
  25:   bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("DepositHook_setTokenSender(ITokenSender)");

/DepositRecord.sol:
  14:   bytes32 public constant SET_GLOBAL_NET_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setGlobalNetDepositCap(uint256)");
  15:   bytes32 public constant SET_USER_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setUserDepositCap(uint256)");
  16:   bytes32 public constant SET_ALLOWED_HOOK_ROLE = keccak256("DepositRecord_setAllowedHook(address)");

/ManagerWithdrawHook.sol:
  13:   bytes32 public constant SET_COLLATERAL_ROLE = keccak256("ManagerWithdrawHook_setCollateral(address)");
  14:   bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("ManagerWithdrawHook_setDepositRecord(address)");
  15:   bytes32 public constant SET_MIN_RESERVE_PERCENTAGE_ROLE = keccak256("ManagerWithdrawHook_setMinReservePercentage(uint256)");

/TokenSender.sol:
  26:   bytes32 public constant SET_PRICE_ROLE = keccak256("TokenSender_setPrice(IUintValue)");
  27:   bytes32 public constant SET_PRICE_MULTIPLIER_ROLE = keccak256("TokenSender_setPriceMultiplier(uint256)");
  28:   bytes32 public constant SET_SCALED_PRICE_LOWER_BOUND_ROLE = keccak256("TokenSender_setScaledPriceLowerBound(uint256)");
  29:   bytes32 public constant SET_ALLOWED_MSG_SENDERS_ROLE = keccak256("TokenSender_setAllowedMsgSenders(IAccountList)");

/WithdrawHook.sol:
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
### [N-16] Mark visibility of initialize(...) functions as ``external``

```solidity
/Collateral.sol:
  33  
  34:   function initialize(string memory _name, string memory _symbol) public initializer {
  35      __SafeAccessControlEnumerable_init();


/PrePOMarketFactory.sol:
  15  
  16:   function initialize() public initializer { OwnableUpgradeable.__Ownable_init(); }
  17 
```

**Description:**
If someone wants to extend via inheritance, it might make more sense that the overridden initialize(...) function calls the internal {...}_init function, not the parent public initialize(...) function.

External instead of public would give more the sense of the initialize(...) functions to behave like a constructor (only called on deployment, so should only be called externally)

Security point of view, it might be safer so that it cannot be called internally by accident in the child contract 

It might cost a bit less gas to use external over public 


It is possible to override a function from external to public (= "opening it up") ✅
but it is not possible to override a function from public to external (= "narrow it down"). ❌

For above reasons you can change initialize(...) to external


https://github.com/OpenZeppelin/openzeppelin-contracts/issues/3750


### [S-01] Make the Test Context with Solidity

It's crucial to write tests with possibly 100% coverage for smart contract systems.

It is recommended to write appropriate tests for all possible code streams and especially for extreme cases.

But the other important point is the test context, tests written with solidity are safer, it is recommended to focus on tests with Foundry

### [S-02] Project Upgrade and Stop Scenario should be

At the start of the project, the system may need to be stopped or upgraded, I suggest you have a script beforehand and add it to the documentation.
This can also be called an " EMERGENCY STOP (CIRCUIT BREAKER) PATTERN ".

https://github.com/maxwoe/solidity_patterns/blob/master/security/EmergencyStop.sol


### [S-03] Generate perfect code headers every time

**Description:**
I recommend using header for Solidity code layout and readability

https://github.com/transmissions11/headers

```js
/*//////////////////////////////////////////////////////////////
                           TESTING 123
//////////////////////////////////////////////////////////////*/
```
