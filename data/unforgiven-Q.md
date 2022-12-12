[[1]] functions `reset()` and `set()` in AccountList doesn't delete storage values, by removing unused storage variables gas can be saved. when resetIndex increase, the previous data related to old resetIndex can be deleted from contract storage.
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/AccountList.sol#L24-L33
