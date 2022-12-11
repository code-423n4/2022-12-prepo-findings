# Use decimal separator or scientific notation in number literals

This improves readability and avoids developer mistakes

```diff
diff --git a/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol b/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol
index da871ae..ee9ad1d 100644
--- a/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol
+++ b/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol
@@ -9,12 +9,18 @@ contract ManagerWithdrawHook is IManagerWithdrawHook, SafeAccessControlEnumerabl
   IDepositRecord private depositRecord;
   uint256 private minReservePercentage;
 
-  uint256 public constant PERCENT_DENOMINATOR = 1000000;
+  uint256 public constant PERCENT_DENOMINATOR = 1_000_000; // or 1e6

```
