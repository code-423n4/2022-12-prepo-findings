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
