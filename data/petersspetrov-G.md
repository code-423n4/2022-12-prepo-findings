x = x - y; costs less gas than x -= y;, same for gathering You can replace all -= and += encounters to save gas

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/DepositRecord.sol#L36

globalNetDepositAmount -= _amount;  to be    globalNetDepositAmount = globalNetDepositAmount - _amount;
