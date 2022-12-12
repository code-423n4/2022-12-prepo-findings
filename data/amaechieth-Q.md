Missing Events For Owner/Admin Functions that change critical parameters.

Some of these setter functions emit events to record these changes on-chain for off-chain monitors/tools/interfaces to register the updates and react if necessary.

The impact of this is that, if a malicious owner changes the critical addresses or values that significantly change the security posture/perception of the protocol. No events are emitted and users lose funds/confidence. The protocol takes a reputation hit.

Recommend to consider emitting events when these addresses/values are updated. This will be more transparent and it will make it easier to keep track of the status of the system.

Similar to https://blog.openzeppelin.com/uma-audit-phase-4/

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L80

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/DepositRecord.sol#L28

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/AccountList.sol#L13

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/AccountList.sol#L24