## Use of named returns for local variables saves gas
You can have further advantages in term of gas cost by simply using named return values as temporary local variable.

For instance, the code block instance below may be refactored as follows:

[File: AccountListCaller.sol#L15-L17](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol#L15-L17)

```
-  function getAccountList() external view override returns (IAccountList) {
-    return _accountList;
+  function getAccountList() external view override returns (IAccountList accountList) {
+    accountList = _accountList;
```
## State Variables Repeatedly Read Should be Cached
SLOADs cost 100 gas each after the 1st one whereas MLOADs/MSTOREs only incur 3 gas each. As such, storage values read multiple times should be cached in the stack memory the first time (costing only 1 SLOAD) and then re-read from this cache to avoid multiple SLOADs.

For instance, `_requiredScore` in the instance below should be cached as follows:

[File: NFTScoreRequirement.sol#L13-L15](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L13-L15)

```
+    uint256 requiredScore = _requiredScore; // 1 SLOAD
-    return _requiredScore == 0 || getAccountScore(account) >= _requiredScore;
+    return requiredScore == 0 || getAccountScore(account) >= requiredScore; // 2 MLOADs
```
## += and -= Costs More Gas
`+=` generally costs 22 more gas than writing out the assigned equation explicitly. The amount of gas wasted can be quite sizable when repeatedly operated in a loop.

For instance, the `+=` instance below may be refactored as follows:

[File: NFTScoreRequirement.sol#L60](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L60)

```
-      score += IERC721(collection).balanceOf(account) > 0 ? collectionScore : 0;
+      score = score + IERC721(collection).balanceOf(account) > 0 ? collectionScore : 0;
```
## Non-strict inequalities are cheaper than strict ones
In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =).

As an example, consider replacing `>=` with the strict counterpart `>` in the following inequality instance:

[File: NFTScoreRequirement.sol#L14](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L14)

```
-    return _requiredScore == 0 || getAccountScore(account) >= _requiredScore;
+    return _requiredScore == 0 || getAccountScore(account) > _requiredScore - 1;
```
Similarly, consider replacing `<=` with the strict counterpart `<` in the following inequality instance, as an example:

[File: Collateral.sol#L91](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L91)

```
-    require(_newDepositFee <= FEE_LIMIT, "exceeds fee limit");
+    require(_newDepositFee < FEE_LIMIT + 1, "exceeds fee limit");
```
## Ternary over `if ... else`
Using ternary operator instead of the if else statement saves gas.

For instance, the code block below may be refactored as follows:

[File: Collateral.sol#L67-L68](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L67-L68)

```
-    if (withdrawFee > 0) { require(_fee > 0, "fee = 0"); }
-    else { require(_baseTokenAmount > 0, "amount = 0"); }
+    withdrawFee > 0 ? require(_fee > 0, "fee = 0") : require(_baseTokenAmount > 0, "amount = 0");
```
## Payable access control functions costs less gas
Consider marking functions with access control as `payable`. This will save 20 gas on each call by their respective permissible callers for not needing to have the compiler check for `msg.value`.

For instance, the following code line may be refactored as follows:

[File: DepositHook.sol#L43](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L43)

```
-  function hook(address _sender, uint256 _amountBeforeFee, uint256 _amountAfterFee) external override onlyCollateral {
+  function hook(address _sender, uint256 _amountBeforeFee, uint256 _amountAfterFee) external payable override onlyCollateral {
```
## Private function with embedded modifier reduces contract size
Consider having the logic of a modifier embedded through a private function to reduce contract size if need be. A private visibility that saves more gas on function calls than the internal visibility is adopted because the modifier will only be making this call inside the contract.

For instance, the modifier instance below may be refactored as follows:

[File: DepositHook.sol#L27-L30](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L27-L30)

```
+    function _onlyCollateral() private view {
+        require(msg.sender == address(collateral), "msg.sender != collateral");
+    }

    modifier onlyCollateral() {
-      require(msg.sender == address(collateral), "msg.sender != collateral");
+        _onlyCollateral();
        _;
    }
```
## Function order affects gas consumption
The order of function will also have an impact on gas consumption. Because in smart contracts, there is a difference in the order of the functions. Each position will have an extra 22 gas. The order is dependent on method ID. So, if you rename the frequently accessed function to more early method ID, you can save gas cost. Please visit the following site for further information:

https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Activate the optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
solidity: {
  version: "0.8.7",
  settings: {
    optimizer: {
      enabled: true,
      runs: 1000,
    },
  },
},
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:

```
for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI
```
Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past . A high-severity bug in the emscripten -generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

## Use storage instead of memory for structs/arrays
A storage pointer is cheaper since copying a state struct in memory would incur as many SLOADs and MSTOREs as there are slots. In another words, this causes all fields of the struct/array to be read from storage, incurring a Gcoldsload (2100 gas) for each field of the struct/array, and then further incurring an additional MLOAD rather than a cheap stack read. As such, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables will be much cheaper, involving only Gcoldsload for all associated field reads. Read the whole struct/array into a memory variable only when it is being returned by the function, passed into a function that requires memory, or if the array/struct is being read from another memory array/struct.

Here are some of the instances entailed:

// Not displayed due to line too long
[File: DepositTradeHelper.sol#L32](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L32)

## Unused imports
Importing source units that are not referenced in the contract increases the size of deployment. Consider removing them to save some gas. If there was a plan to use them in the future, a comment explaining why they were imported would be helpful.

Here are some of the instances entailed:

[File: PrePOMarketFactory.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol)

```
4: import "./LongShortToken.sol";

6:import "./interfaces/ILongShortToken.sol";
```