## [G - 1 ] State variables can even more  packed 
      (refer permalink below) can be changed as  (uint128 instead of uint256) this can save 1 Sslot ( 20000 gas can saved)
       uint128 private minReservePercentage;
       uint128 public constant PERCENT_DENOMINATOR = 1000000;

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L10-L12

## [G - 2 ] Solidity version can be updated to latest version   
       for more optimization its recommended to use the latest stable version

