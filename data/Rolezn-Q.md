## Summary<a name="Summary">

### Low Risk Issues
| |Issue|Contexts|
|-|:-|:-:|
| [LOW&#x2011;1](#LOW&#x2011;1) | Missing Checks for Address(0x0)  | 1 |
| [LOW&#x2011;2](#LOW&#x2011;2) | Usage of Draft OZ contracts  | 2 |
| [LOW&#x2011;3](#LOW&#x2011;3) | The Contract Should `approve(0)` First | 3 |
| [LOW&#x2011;4](#LOW&#x2011;4) | Use `_safeMint` instead of `_mint` | 1 |
| [LOW&#x2011;5](#LOW&#x2011;5) | Contracts are not using their OZ Upgradeable counterparts | 11 |
| [LOW&#x2011;6](#LOW&#x2011;6) | Missing parameter validation in `constructor` | 5 |
| [LOW&#x2011;7](#LOW&#x2011;7) | The `nonReentrant` modifier should occur before all other modifiers | 2 |

Total: 25 contexts over 7 issues

### Non-critical Issues
| |Issue|Contexts|
|-|:-|:-:|
| [NC&#x2011;1](#NC&#x2011;1) | Critical Changes Should Use Two-step Procedure | 35 |
| [NC&#x2011;2](#NC&#x2011;2) | Use a more recent version of Solidity | 11 |
| [NC&#x2011;3](#NC&#x2011;3) | Missing event for critical parameter change | 12 |
| [NC&#x2011;4](#NC&#x2011;4) | Implementation contract may not be initialized | 6 |
| [NC&#x2011;5](#NC&#x2011;5) | Large multiples of ten should use scientific notation | 7 |
| [NC&#x2011;6](#NC&#x2011;6) | Use of Block.Timestamp | 1 |
| [NC&#x2011;7](#NC&#x2011;7) | Non-usage of specific imports | 15 |
| [NC&#x2011;8](#NC&#x2011;8) | Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant` | 26 |
| [NC&#x2011;9](#NC&#x2011;9) | Use `bytes.concat()` | 5 |
| [NC&#x2011;10](#NC&#x2011;10) | Open TODOs | 1 |
| [NC&#x2011;11](#NC&#x2011;11) | Use Underscores for Number Literals  | 7 |

Total: 126 contexts over 11 issues

## Low Risk Issues

### <a href="#Summary">[LOW&#x2011;1]</a><a name="LOW&#x2011;1"> Missing Checks for Address(0x0) 

Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

#### <ins>Proof Of Concept</ins>


```solidity
85: function setManager: address _newManager
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L85



#### <ins>Recommended Mitigation Steps</ins>

Consider adding explicit zero-address validation on input parameters of address type.



### <a href="#Summary">[LOW&#x2011;2]</a><a name="LOW&#x2011;2"> Usage of Draft OZ contracts

The following contract uses the draft version of the OZ contract, it is recommended to not to use draft versions as they are no finalized and are subject to change.

#### <ins>Proof Of Concept</ins>

```solidity
5 import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-ERC20PermitUpgradeable.sol";
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L5


```solidity
6: import "@openzeppelin/contracts/token/ERC20/extensions/draft-IERC20Permit.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L6


### <a href="#Summary">[LOW&#x2011;3]</a><a name="LOW&#x2011;3"> The Contract Should `approve(0)` First

Some tokens (like USDT L199) do not work when changing the allowance from an existing non-zero allowance value. They must first be approved by zero and then the actual allowance must be approved.

#### <ins>Proof Of Concept</ins>


```solidity
52: baseToken.approve(address(depositHook), _fee);
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L52

```solidity
72: baseToken.approve(address(withdrawHook), _fee);
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L72

```solidity
94: collateral.approve(address(_redeemHook), _expectedFee);
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L94



#### <ins>Recommended Mitigation Steps</ins>

Approve with a zero amount first before setting the actual amount.




### <a href="#Summary">[LOW&#x2011;4]</a><a name="LOW&#x2011;4"> Use `_safeMint` instead of `_mint`

According to openzepplin's ERC721, the use of `_mint` is discouraged, use _safeMint whenever possible.
https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721-_mint-address-uint256-

#### <ins>Proof Of Concept</ins>


```solidity
58: _mint(_recipient, _collateralMintAmount);
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L58



#### <ins>Recommended Mitigation Steps</ins>

Use `_safeMint` whenever possible instead of `_mint`




### <a href="#Summary">[LOW&#x2011;5]</a><a name="LOW&#x2011;5"> Contracts are not using their OZ Upgradeable counterparts

The non-upgradeable standard version of OpenZeppelin’s library are inherited / used by the contracts.
It would be safer to use the upgradeable versions of the library contracts to avoid unexpected behaviour.

#### <ins>Proof of Concept</ins>

```solidity
10: import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L10

```solidity
6: import "@openzeppelin/contracts/token/ERC20/extensions/draft-IERC20Permit.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L6

```solidity
4: import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
5: import "@openzeppelin/contracts/access/Ownable.sol";
6: import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol#L4-L6

```solidity
6: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
7: import "@openzeppelin/contracts/access/Ownable.sol";
8: import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L6-L8

```solidity
7: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L7

```solidity
9: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
10: import "@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L9-L10



#### <ins>Recommended Mitigation Steps</ins>

Where applicable, use the contracts from `@openzeppelin/contracts-upgradeable` instead of `@openzeppelin/contracts`.



### <a href="#Summary">[LOW&#x2011;6]</a><a name="LOW&#x2011;6"> Missing parameter validation in `constructor`

Some parameters of constructors are not checked for invalid values.

#### <ins>Proof Of Concept</ins>

```solidity
42: address _governance
42: address _collateral
42: uint256 _floorValuation
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L42

```solidity
29: constructor(IERC20 _newBaseToken, uint256 _newBaseTokenDecimals)
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L29

```solidity
23: constructor(uint256 _newGlobalNetDepositCap, uint256 _newUserDepositCap)
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L23

```solidity
14: constructor(ICollateral collateral, ISwapRouter swapRouter)
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L14


```solidity
31: constructor(IERC20Metadata outputToken)
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L31



#### <ins>Recommended Mitigation Steps</ins>

Validate the parameters.



### <a href="#Summary">[LOW&#x2011;7]</a><a name="LOW&#x2011;7"> The `nonReentrant` modifier should occur before all other modifiers

Currently the `nonReentrant` modifier is not the first to occur, it should occur before all other modifiers.

#### <ins>Proof Of Concept</ins>


```solidity
80: function managerWithdraw(uint256 _amount) external override onlyRole(MANAGER_WITHDRAW_ROLE) nonReentrant {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L80

```solidity
22: function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22



#### <ins>Recommended Mitigation Steps</ins>

Re-sort modifiers so that the `nonReentrant` modifier occurs first.



## Non Critical Issues

### <a href="#Summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> Critical Changes Should Use Two-step Procedure

The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

#### <ins>Proof Of Concept</ins>

```solidity
85: function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L85

```solidity
90: function setDepositFee(uint256 _newDepositFee) external override onlyRole(SET_DEPOSIT_FEE_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L90

```solidity
96: function setWithdrawFee(uint256 _newWithdrawFee) external override onlyRole(SET_WITHDRAW_FEE_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L96

```solidity
102: function setDepositHook(ICollateralHook _newDepositHook) external override onlyRole(SET_DEPOSIT_HOOK_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L102

```solidity
107: function setWithdrawHook(ICollateralHook _newWithdrawHook) external override onlyRole(SET_WITHDRAW_HOOK_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L107

```solidity
112: function setManagerWithdrawHook(ICollateralHook _newManagerWithdrawHook) external override onlyRole(SET_MANAGER_WITHDRAW_HOOK_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L112

```solidity
54: function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L54

```solidity
59: function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L59

```solidity
64: function setDepositsAllowed(bool _newDepositsAllowed) external override onlyRole(SET_DEPOSITS_ALLOWED_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L64

```solidity
69: function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) { super.setAccountList(accountList); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L69

```solidity
71: function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) { super.setRequiredScore(_newRequiredScore); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L71

```solidity
73: function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L73

```solidity
77: function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) { super.setTreasury(_treasury); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L77

```solidity
79: function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L79

```solidity
40: function setGlobalNetDepositCap(uint256 _newGlobalNetDepositCap) external override onlyRole(SET_GLOBAL_NET_DEPOSIT_CAP_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L40

```solidity
45: function setUserDepositCap(uint256 _newUserDepositCap) external override onlyRole(SET_USER_DEPOSIT_CAP_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L45

```solidity
50: function setAllowedHook(address _hook, bool _allowed) external override onlyRole(SET_ALLOWED_HOOK_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L50

```solidity
19: function setCollateral(ICollateral _newCollateral) external override onlyRole(SET_COLLATERAL_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L19

```solidity
24: function setDepositRecord(IDepositRecord _newDepositRecord) external override onlyRole(SET_DEPOSIT_RECORD_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L24

```solidity
29: function setMinReservePercentage(uint256 _newMinReservePercentage) external override onlyRole(SET_MIN_RESERVE_PERCENTAGE_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L29

```solidity
18: function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L18

```solidity
20: function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L20

```solidity
109: function setMintHook(IMarketHook mintHook) external override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L109

```solidity
114: function setRedeemHook(IMarketHook redeemHook) external override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L114

```solidity
119: function setFinalLongPayout(uint256 _finalLongPayout) external override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L119

```solidity
126: function setRedemptionFee(uint256 _redemptionFee) external override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L126

```solidity
36: function setCollateralValidity(address _collateral, bool _validity) external override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L36

```solidity
26: function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L26

```solidity
28: function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L28

```solidity
30: function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L30

```solidity
32: function setTokenSender(ITokenSender tokenSender) public override onlyOwner { super.setTokenSender(tokenSender); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L32

```solidity
45: function setPrice(IUintValue price) external override onlyRole(SET_PRICE_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L45

```solidity
50: function setPriceMultiplier(uint256 multiplier) external override onlyRole(SET_PRICE_MULTIPLIER_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L50

```solidity
55: function setScaledPriceLowerBound(uint256 lowerBound) external override onlyRole(SET_SCALED_PRICE_LOWER_BOUND_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L55

```solidity
60: function setAllowedMsgSenders(IAccountList allowedMsgSenders) public override onlyRole(SET_ALLOWED_MSG_SENDERS_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L60



#### <ins>Recommended Mitigation Steps</ins>

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.



### <a href="#Summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> Use a more recent version of Solidity

<a href="https://blog.soliditylang.org/2022/02/16/solidity-0.8.12-release-announcement/">0.8.12</a>: 
string.concat() instead of abi.encodePacked(<str>,<str>)

<a href="https://blog.soliditylang.org/2022/03/16/solidity-0.8.13-release-announcement/">0.8.13</a>: 
Ability to use using for with a list of free functions

<a href="https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/">0.8.14</a>:

ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against calldatasize() in all cases.
Override Checker: Allow changing data location for parameters only when overriding external functions.

<a href="https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/">0.8.15</a>:

Code Generation: Avoid writing dirty bytes to storage when copying bytes arrays.
Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

<a href="https://blog.soliditylang.org/2022/08/08/solidity-0.8.16-release-announcement/">0.8.16</a>:

Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

<a href="https://blog.soliditylang.org/2022/09/08/solidity-0.8.17-release-announcement/">0.8.17</a>:

Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.

#### <ins>Proof Of Concept</ins>


```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L2

```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L2

```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L2

```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L2

```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol#L2

```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L2

```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L2

```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L2

```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L2

```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L2

```solidity
pragma solidity =0.8.7;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L2



#### <ins>Recommended Mitigation Steps</ins>

Consider updating to a more recent solidity version.



### <a href="#Summary">[NC&#x2011;3]</a><a name="NC&#x2011;3"> Missing event for critical parameter change

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

#### <ins>Proof Of Concept</ins>


```solidity
69: function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) { super.setAccountList(accountList); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L69

```solidity
71: function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) { super.setRequiredScore(_newRequiredScore); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L71

```solidity
73: function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) { super.setCollectionScores(_collections, _scores); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L73

```solidity
77: function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) { super.setTreasury(_treasury); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L77

```solidity
79: function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L79

```solidity
18: function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L18

```solidity
20: function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L20

```solidity
26: function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L26

```solidity
28: function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L28

```solidity
30: function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L30

```solidity
32: function setTokenSender(ITokenSender tokenSender) public override onlyOwner { super.setTokenSender(tokenSender); }
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L32

```solidity
60: function setAllowedMsgSenders(IAccountList allowedMsgSenders) public override onlyRole(SET_ALLOWED_MSG_SENDERS_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L60





### <a href="#Summary">[NC&#x2011;4]</a><a name="NC&#x2011;4"> Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

#### <ins>Proof Of Concept</ins>


```solidity
29: constructor(IERC20 _newBaseToken, uint256 _newBaseTokenDecimals)
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L29

```solidity
23: constructor(uint256 _newGlobalNetDepositCap, uint256 _newUserDepositCap)
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L23

```solidity
14: constructor(ICollateral collateral, ISwapRouter swapRouter)
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L14

```solidity
9: constructor(string memory name_, string memory symbol_) ERC20(name_, symbol_)
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol#L9

```solidity
42: constructor(address _governance, address _collateral, ILongShortToken _longToken, ILongShortToken _shortToken, uint256 _floorLongPayout, uint256 _ceilingLongPayout, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime)
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L42

```solidity
31: constructor(IERC20Metadata outputToken)
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L31





### <a href="#Summary">[NC&#x2011;5]</a><a name="NC&#x2011;5"> Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 100000), for better code readability.

#### <ins>Proof Of Concept</ins>


```solidity
19: uint256 public constant FEE_DENOMINATOR = 1000000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L19

```solidity
20: uint256 public constant FEE_LIMIT = 100000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L20

```solidity
12: uint24 public constant override POOL_FEE_TIER = 10000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L12

```solidity
12: uint256 public constant PERCENT_DENOMINATOR = 1000000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L12

```solidity
30: uint256 private constant FEE_DENOMINATOR = 1000000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L30

```solidity
31: uint256 private constant FEE_LIMIT = 100000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L31

```solidity
25: uint256 public constant MULTIPLIER_DENOMINATOR = 10000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L25





### <a href="#Summary">[NC&#x2011;6]</a><a name="NC&#x2011;6"> Use of Block.Timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
References: SWC ID: 116

#### <ins>Proof Of Concept</ins>


```solidity
44: require(_expiryTime > block.timestamp, "Invalid expiry");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L44



#### <ins>Recommended Mitigation Steps</ins>
Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.



### <a href="#Summary">[NC&#x2011;7]</a><a name="NC&#x2011;7"> Non-usage of specific imports

The current form of relative path import is not recommended for use because it can unpredictably pollute the namespace.
Instead, the Solidity docs recommend specifying imported symbols explicitly.
https://docs.soliditylang.org/en/v0.8.15/layout-of-source-files.html#importing-other-source-files

A good example:
```solidity
import {OwnableUpgradeable} from "openzeppelin-contracts-upgradeable/contracts/access/OwnableUpgradeable.sol";
import {SafeTransferLib} from "solmate/utils/SafeTransferLib.sol";
import {SafeCastLib} from "solmate/utils/SafeCastLib.sol";
import {ERC20} from "solmate/tokens/ERC20.sol";
import {IProducer} from "src/interfaces/IProducer.sol";
import {GlobalState, UserState} from "src/Common.sol";
```

#### <ins>Proof Of Concept</ins>


```solidity
4: import "./interfaces/ICollateral.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L4

```solidity
4: import "./interfaces/IDepositHook.sol";
5: import "./interfaces/IDepositRecord.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L4-L5

```solidity
4: import "./interfaces/IDepositRecord.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L4

```solidity
4: import "./interfaces/IDepositTradeHelper.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L4

```solidity
4: import "./interfaces/IManagerWithdrawHook.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L4

```solidity
4: import "./interfaces/IMarketHook.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L4

```solidity
4: import "./interfaces/ILongShortToken.sol";
5: import "./interfaces/IPrePOMarket.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L4-L5

```solidity
4: import "./LongShortToken.sol";
5: import "./PrePOMarket.sol";
6: import "./interfaces/ILongShortToken.sol";
10: import "./interfaces/IPrePOMarketFactory.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L4

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L5

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L6

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L10



```solidity
4: import "./interfaces/IPrePOMarket.sol";
5: import "./interfaces/IMarketHook.sol";

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L4-L5




#### <ins>Recommended Mitigation Steps</ins>

Use specific imports syntax per solidity docs recommendation.



### <a href="#Summary">[NC&#x2011;8]</a><a name="NC&#x2011;8"> Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

While it doesn't save any gas because the compiler knows that developers often make this mistake, it's still best to use the right tool for the task at hand. There is a difference between `constant` variables and `immutable` variables, and they should each be used in their appropriate contexts. `constants` should be used for literal values written into the code, and `immutable` variables should be used for expressions, or values calculated in, or passed into the constructor.

#### <ins>Proof Of Concept</ins>

```solidity
21: bytes32 public constant MANAGER_WITHDRAW_ROLE = keccak256("Collateral_managerWithdraw(uint256)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L21

```solidity
22: bytes32 public constant SET_MANAGER_ROLE = keccak256("Collateral_setManager(address)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L22

```solidity
23: bytes32 public constant SET_DEPOSIT_FEE_ROLE = keccak256("Collateral_setDepositFee(uint256)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L23

```solidity
24: bytes32 public constant SET_WITHDRAW_FEE_ROLE = keccak256("Collateral_setWithdrawFee(uint256)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L24

```solidity
25: bytes32 public constant SET_DEPOSIT_HOOK_ROLE = keccak256("Collateral_setDepositHook(ICollateralHook)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L25

```solidity
26: bytes32 public constant SET_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setWithdrawHook(ICollateralHook)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L26

```solidity
27: bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L27

```solidity
17: bytes32 public constant SET_COLLATERAL_ROLE = keccak256("DepositHook_setCollateral(address)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L17

```solidity
18: bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("DepositHook_setDepositRecord(address)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L18

```solidity
19: bytes32 public constant SET_DEPOSITS_ALLOWED_ROLE = keccak256("DepositHook_setDepositsAllowed(bool)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L19

```solidity
20: bytes32 public constant SET_ACCOUNT_LIST_ROLE = keccak256("DepositHook_setAccountList(IAccountList)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L20

```solidity
21: bytes32 public constant SET_REQUIRED_SCORE_ROLE = keccak256("DepositHook_setRequiredScore(uint256)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L21

```solidity
22: bytes32 public constant SET_COLLECTION_SCORES_ROLE = keccak256("DepositHook_setCollectionScores(IERC721[],uint256[])");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L22

```solidity
23: bytes32 public constant REMOVE_COLLECTIONS_ROLE = keccak256("DepositHook_removeCollections(IERC721[])");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L23

```solidity
24: bytes32 public constant SET_TREASURY_ROLE = keccak256("DepositHook_setTreasury(address)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L24

```solidity
25: bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("DepositHook_setTokenSender(ITokenSender)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L25

```solidity
14: bytes32 public constant SET_GLOBAL_NET_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setGlobalNetDepositCap(uint256)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L14

```solidity
15: bytes32 public constant SET_USER_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setUserDepositCap(uint256)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L15

```solidity
16: bytes32 public constant SET_ALLOWED_HOOK_ROLE = keccak256("DepositRecord_setAllowedHook(address)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L16

```solidity
13: bytes32 public constant SET_COLLATERAL_ROLE = keccak256("ManagerWithdrawHook_setCollateral(address)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L13

```solidity
14: bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("ManagerWithdrawHook_setDepositRecord(address)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L14

```solidity
15: bytes32 public constant SET_MIN_RESERVE_PERCENTAGE_ROLE = keccak256("ManagerWithdrawHook_setMinReservePercentage(uint256)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L15

```solidity
26: bytes32 public constant SET_PRICE_ROLE = keccak256("TokenSender_setPrice(IUintValue)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L26

```solidity
27: bytes32 public constant SET_PRICE_MULTIPLIER_ROLE = keccak256("TokenSender_setPriceMultiplier(uint256)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L27

```solidity
28: bytes32 public constant SET_SCALED_PRICE_LOWER_BOUND_ROLE = keccak256("TokenSender_setScaledPriceLowerBound(uint256)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L28

```solidity
29: bytes32 public constant SET_ALLOWED_MSG_SENDERS_ROLE = keccak256("TokenSender_setAllowedMsgSenders(IAccountList)");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L29





### <a href="#Summary">[NC&#x2011;9]</a><a name="NC&#x2011;9"> Use `bytes.concat()`

Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

#### <ins>Proof Of Concept</ins>


```solidity
26: bytes32 _salt = keccak256(abi.encodePacked(_longToken, _shortToken)
42: string memory _longTokenName = string(abi.encodePacked("LONG", " ", _tokenNameSuffix)
43: string memory _shortTokenName = string(abi.encodePacked("SHORT", " ", _tokenNameSuffix)
44: string memory _longTokenSymbol = string(abi.encodePacked("L", "_", _tokenSymbolSuffix)
45: string memory _shortTokenSymbol = string(abi.encodePacked("S", "_", _tokenSymbolSuffix)

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L26

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L42

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L43

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L44

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L45






#### <ins>Recommended Mitigation Steps</ins>

Use `bytes.concat()` and upgrade to at least Solidity version 0.8.4 if required. 



### <a href="#Summary">[NC&#x2011;10]</a><a name="NC&#x2011;10"> Open TODOs

An open TODO is present. It is recommended to avoid open TODOs as they may indicate programming errors that still need to be fixed.

#### <ins>Proof Of Concept</ins>


```solidity
15: WithdrawERC20, // TODO: Access control when WithdrawERC20 updated
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L15






### <a href="#Summary">[NC&#x2011;11]</a><a name="NC&#x2011;11"> Use Underscores for Number Literals

i.e. for 1000000 use 1_000_000 to improve readability

#### <ins>Proof Of Concept</ins>


```solidity
19: uint256 public constant FEE_DENOMINATOR = 1000000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L19

```solidity
20: uint256 public constant FEE_LIMIT = 100000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L20

```solidity
12: uint24 public constant override POOL_FEE_TIER = 10000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L12

```solidity
12: uint256 public constant PERCENT_DENOMINATOR = 1000000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L12

```solidity
30: uint256 private constant FEE_DENOMINATOR = 1000000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L30

```solidity
31: uint256 private constant FEE_LIMIT = 100000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L31

```solidity
25: uint256 public constant MULTIPLIER_DENOMINATOR = 10000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L25






