## Summary<a name="Summary">

### Gas Optimizations
| |Issue|Contexts|Estimated Gas Saved|
|-|:-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate | 2 | - |
| [GAS&#x2011;2](#GAS&#x2011;2) | Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function | 2 | 56 |
| [GAS&#x2011;3](#GAS&#x2011;3) | `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables | 3 | - |
| [GAS&#x2011;4](#GAS&#x2011;4) | `require()`/`revert()` Strings Longer Than 32 Bytes Cost Extra Gas | 4 | - |
| [GAS&#x2011;5](#GAS&#x2011;5) | Use calldata instead of memory for function parameters | 2 | 600 |
| [GAS&#x2011;6](#GAS&#x2011;6) | Public Functions To External | 15 | - |
| [GAS&#x2011;7](#GAS&#x2011;7) | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 1 | - |
| [GAS&#x2011;8](#GAS&#x2011;8) | Optimize names to save gas | 10 | 220 |
| [GAS&#x2011;9](#GAS&#x2011;9) | Using fixed bytes is cheaper than using `string` | 4 | - |
| [GAS&#x2011;10](#GAS&#x2011;10) | `internal` functions only called once can be inlined to save gas | 1 | - |
| [GAS&#x2011;11](#GAS&#x2011;11) | Setting the `constructor` to `payable` | 6 | 78 |
| [GAS&#x2011;12](#GAS&#x2011;12) | Functions guaranteed to revert when called by normal users can be marked `payable` | 47 | 987 |
| [GAS&#x2011;13](#GAS&#x2011;13) | Using `unchecked` blocks to save gas | 8 | 1088 |

Total: 105 contexts over 13 issues

## Gas Optimizations

### <a href="#Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

i.e. Use:
```solidity
struct userDepositRecord {
	uint256 userToDeposits;
	bool allowedHooks;
}
```

#### <ins>Proof Of Concept</ins>


```solidity
11: mapping(address => uint256) private userToDeposits;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L11

```solidity
12: mapping(address => bool) private allowedHooks;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L12





#### <a href="#Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function

Saves deployment costs

#### <ins>Proof Of Concept</ins>

```solidity
47: (_fee > 0, "fee = 0");
67: (_fee > 0, "fee = 0");

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L47

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L67








### <a href="#Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables

#### <ins>Proof Of Concept</ins>


```solidity
31: globalNetDepositAmount += _amount;
32: userToDeposits[_sender] += _amount;

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L31-L32

```solidity
36: if (globalNetDepositAmount > _amount) { globalNetDepositAmount -= _amount; }

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L36





### <a href="#Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> `require()`/`revert()` Strings Longer Than 32 Bytes Cost Extra Gas

#### <ins>Proof Of Concept</ins>


```solidity
17: require(collateral.getReserve() - _amountAfterFee >= getMinReserve(), "reserve would fall below minimum");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L17

```solidity
85: require(_longAmount == _shortAmount, "Long and Short must be equal");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L85

```solidity
120: require(_finalLongPayout >= floorLongPayout, "Payout cannot be below floor");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L120

```solidity
121: require(_finalLongPayout <= ceilingLongPayout, "Payout cannot exceed ceiling");
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L121




### <a href="#Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> Use calldata instead of memory for function parameters

In some cases, having function arguments in calldata instead of
memory is more optimal.

Consider the following generic example:
```
contract C {
	function add(uint[] memory arr) external returns (uint sum) {
		uint length = arr.length;
		for (uint i = 0; i < arr.length; i++) {
		    sum += arr[i];
		}
	}
}
```
In the above example, the dynamic array arr has the storage location
memory. When the function gets called externally, the array values are
kept in calldata and copied to memory during ABI decoding (using the
opcode calldataload and mstore). And during the for loop, arr[i]
accesses the value in memory using a mload. However, for the above
example this is inefficient. Consider the following snippet instead:
```
contract C {
	function add(uint[] calldata arr) external returns (uint sum) {
		uint length = arr.length;
		for (uint i = 0; i < arr.length; i++) {
		    sum += arr[i];
		}
	}
}
```
In the above snippet, instead of going via memory, the value is directly
read from calldata using calldataload. That is, there are no
intermediate memory operations that carries this value.

Gas savings: In the former example, the ABI decoding begins with
copying value from calldata to memory in a for loop. Each iteration
would cost at least 60 gas. In the latter example, this can be
completely avoided. This will also reduce the number of instructions and
therefore reduces the deploy time cost of the contract.

In short, use calldata instead of memory if the function argument
is only read.

Note that in older Solidity versions, changing some function arguments
from memory to calldata may cause "unimplemented feature error".
This can be avoided by using a newer (0.8.*) Solidity compiler.

Examples
Note: The following pattern is prevalent in the codebase:
```
function f(bytes memory data) external {
	(...) = abi.decode(data, (..., types, ...));
}
```
Here, changing to bytes calldata will decrease the gas. The total
savings for this change across all such uses would be quite
significant.

#### <ins>Proof Of Concept</ins>


```solidity
function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L73

```solidity
function removeCollections(IERC721[] memory _collections) public override onlyRole(REMOVE_COLLECTIONS_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L75






### <a href="#Summary">[GAS&#x2011;6]</a><a name="GAS&#x2011;6"> Public Functions To External

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

#### <ins>Proof Of Concept</ins>


```solidity
function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L69

```solidity
function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L71

```solidity
function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L73

```solidity
function removeCollections(IERC721[] memory _collections) public override onlyRole(REMOVE_COLLECTIONS_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L75

```solidity
function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L77

```solidity
function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L79

```solidity
function getMinReserve() public view override returns (uint256) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L41

```solidity
function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L18

```solidity
function setAccountList(IAccountList accountList) public virtual override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L20

```solidity
function initialize() public initializer {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L16

```solidity
function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L26

```solidity
function setAccountList(IAccountList accountList) public virtual override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L28

```solidity
function setTreasury(address _treasury) public override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L30

```solidity
function setTokenSender(ITokenSender tokenSender) public override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L32

```solidity
function setAllowedMsgSenders(IAccountList allowedMsgSenders) public override onlyRole(SET_ALLOWED_MSG_SENDERS_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L60






### <a href="#Summary">[GAS&#x2011;7]</a><a name="GAS&#x2011;7"> Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

#### <ins>Proof Of Concept</ins>


```solidity
12: uint24 public constant override POOL_FEE_TIER = 10000;
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L12







### <a href="#Summary">[GAS&#x2011;8]</a><a name="GAS&#x2011;8"> Optimize names to save gas

Contracts most called functions could simply save gas by function ordering via Method ID. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because 22 gas are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

See more <a href="https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92">here</a>

#### <ins>Proof Of Concept</ins>

```solidity
File: /prepo202212\prepo-monorepo\apps\smart-contracts\core\contracts\Collateral.sol
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol

```solidity
File: /prepo202212\prepo-monorepo\apps\smart-contracts\core\contracts\DepositHook.sol
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol

```solidity
File: /prepo202212\prepo-monorepo\apps\smart-contracts\core\contracts\DepositRecord.sol
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol

```solidity
File: /prepo202212\prepo-monorepo\apps\smart-contracts\core\contracts\DepositTradeHelper.sol
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol

```solidity
File: /prepo202212\prepo-monorepo\apps\smart-contracts\core\contracts\LongShortToken.sol
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol

```solidity
File: /prepo202212\prepo-monorepo\apps\smart-contracts\core\contracts\ManagerWithdrawHook.sol
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

```solidity
File: /prepo202212\prepo-monorepo\apps\smart-contracts\core\contracts\MintHook.sol
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol

```solidity
File: /prepo202212\prepo-monorepo\apps\smart-contracts\core\contracts\PrePOMarket.sol
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol

```solidity
File: /prepo202212\prepo-monorepo\apps\smart-contracts\core\contracts\PrePOMarketFactory.sol
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol

```solidity
File: /prepo202212\prepo-monorepo\apps\smart-contracts\core\contracts\RedeemHook.sol
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol



#### <ins>Recommended Mitigation Steps</ins>
Find a lower method ID name for the most called functions for example Call() vs. Call1() is cheaper by 22 gas
For example, the function IDs in the Gauge.sol contract will be the most used; A lower method ID may be given.




### <a href="#Summary">[GAS&#x2011;9]</a><a name="GAS&#x2011;9"> Using fixed bytes is cheaper than using `string`

As a rule of thumb, use `bytes` for arbitrary-length raw byte data and string for arbitrary-length `string` (UTF-8) data. If you can limit the length to a certain number of bytes, always use one of `bytes1` to `bytes32` because they are much cheaper.

#### <ins>Proof Of Concept</ins>


```solidity
42: string memory _longTokenName = string(abi.encodePacked("LONG", " ", _tokenNameSuffix));
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L42

```solidity
43: string memory _shortTokenName = string(abi.encodePacked("SHORT", " ", _tokenNameSuffix));
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L43

```solidity
44: string memory _longTokenSymbol = string(abi.encodePacked("L", "_", _tokenSymbolSuffix));
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L44

```solidity
45: string memory _shortTokenSymbol = string(abi.encodePacked("S", "_", _tokenSymbolSuffix));
```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L45






### <a href="#Summary">[GAS&#x2011;10]</a><a name="GAS&#x2011;10"> `internal` functions only called once can be inlined to save gas

#### <ins>Proof Of Concept</ins>

```solidity
41: function _createPairTokens

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41






### <a href="#Summary">[GAS&#x2011;11]</a><a name="GAS&#x2011;11"> Setting the `constructor` to `payable`

Saves ~13 gas per instance

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





### <a href="#Summary">[GAS&#x2011;12]</a><a name="GAS&#x2011;12"> Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier or require such as onlyOwner/onlyX is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

#### <ins>Proof Of Concept</ins>

```solidity
80: function managerWithdraw(uint256 _amount) external override onlyRole(MANAGER_WITHDRAW_ROLE) nonReentrant {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L80

```solidity
85: function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
112: function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L85

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L112



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
43: function hook(address _sender, uint256 _amountBeforeFee, uint256 _amountAfterFee) external override onlyCollateral {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L43

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
69: function setAccountList(IAccountList accountList) public override onlyRole(SET_ACCOUNT_LIST_ROLE) {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L69

```solidity
71: function setRequiredScore(uint256 _newRequiredScore) public override onlyRole(SET_REQUIRED_SCORE_ROLE) {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L71

```solidity
73: function setCollectionScores(IERC721[] memory _collections, uint256[] memory _scores) public override onlyRole(SET_COLLECTION_SCORES_ROLE) {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L73

```solidity
75: function removeCollections(IERC721[] memory _collections) public override onlyRole(REMOVE_COLLECTIONS_ROLE) {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L75

```solidity
77: function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L77

```solidity
79: function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L79

```solidity
28: function recordDeposit(address _sender, uint256 _amount) external override onlyAllowedHooks {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L28

```solidity
35: function recordWithdrawal(uint256 _amount) external override onlyAllowedHooks {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L35

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
11: function mint(address _recipient, uint256 _amount) external onlyOwner {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol#L11

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
16: function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L16

```solidity
18: function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol#L18

```solidity
20: function setAccountList(IAccountList accountList) public virtual override onlyOwner {

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
22: function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22

```solidity
36: function setCollateralValidity(address _collateral, bool _validity) external override onlyOwner {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L36

```solidity
17: function hook(address sender, uint256 amountBeforeFee, uint256 amountAfterFee) external virtual override onlyAllowedMsgSenders {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L17

```solidity
26: function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L26

```solidity
28: function setAccountList(IAccountList accountList) public virtual override onlyOwner {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L28

```solidity
30: function setTreasury(address _treasury) public override onlyOwner {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L30

```solidity
32: function setTokenSender(ITokenSender tokenSender) public override onlyOwner {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L32

```solidity
36: function send(address recipient, uint256 unconvertedAmount) external override onlyAllowedMsgSenders {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L36

```solidity
45: function setPrice(IUintValue price) external override onlyRole(SET_PRICE_ROLE) {
50: function setPrice(IUintValue price) external override onlyRole(SET_PRICE_ROLE) {

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L45

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L50



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
Functions guaranteed to revert when called by normal users can be marked payable.



### <a href="#Summary">[GAS&#x2011;13]</a><a name="GAS&#x2011;13"> Using `unchecked` blocks to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an `unchecked` block

#### <ins>Proof Of Concept</ins>

```solidity
50: uint256 _amountAfterFee = _amount - _fee;

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L50

```solidity
70: uint256 _baseTokenAmountAfterFee = _baseTokenAmount - _fee;

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L70

```solidity
47: uint256 _fee = _amountBeforeFee - _amountAfterFee;

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L47

```solidity
82: uint256 _shortPayout = MAX_PAYOUT - finalLongPayout;
96: _redeemHook.hook(msg.sender, _collateralAmount, _collateralAmount - _expectedFee);
97: _actualFee = _collateralAllowanceBefore - collateral.allowance(address(this), address(_redeemHook));
103: uint256 _collateralAfterFee = _collateralAmount - _actualFee;

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L82

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L96

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L97

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L103



```solidity
19: uint256 fee = amountBeforeFee - amountAfterFee;

```

https://github.com/prepo-io/prepo-monorepo/tree/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L19





