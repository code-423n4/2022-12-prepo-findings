1.All contracts should have a latest solidity pragma ,is currently 0.8.17
 Also use latest Solidity version to get all compiler features, bugfixes and optimizations.

================================================================================================

2.Index event fields make the field more quickly accessible to off-chain tools that parse events.
 However, note that each index field costs extra gas during emission, so it’s not necessarily best to 
 index the maximum allowed per event (three fields). Each event should use three indexed fields if there 
 are three or more fields, and gas usage is not particularly of concern for the events in question. 
 If there are fewer than three fields, all of the fields should be indexed.
 
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/IAccountListCaller.sol#L15

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/IAllowedMsgSenders.sol#L19

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol#L17

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/ITokenSender.sol#L17

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/ITokenSender.sol#L23

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/ITokenSender.sol#L29

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol#L17

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol#L23



================================================================================================

3.Missing non-zero address checks for IERC20 _newBaseToken in the constructor on contract Collateral.sol

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L29-L32

================================================================================================

4 Missing non-zero address checks for address _governance and  address _collateral in the constructor on contract PrePOMarket.sol
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarket.sol#L42-L63

================================================================================================

5.USE LATEST OPEN ZEPPELIN CONTRACTS

The last version is v4.8.0 project use "manifestVersion": "3.2",

================================================================================================

6.USE NAMED IMPORTS INSTEAD OF PLAIN `IMPORT

You use regular imports on lines:
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/DepositHook.sol#L4-L10
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/DepositRecord.sol#L4-L5
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L4-L6
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L4-L5
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/MintHook.sol#L4-L8
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarket.sol#L4-L8
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L4-L10
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/RedeemHook.sol#L4-L9
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/TokenSender.sol#L4-L10
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/WithdrawHook.sol#L4-L7

import "./interfaces/IDepositTradeHelper.sol";
import "prepo-shared-contracts/contracts/SafeOwnable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/draft-IERC20Permit.sol";


Instead of this, use named imports as you do on for example;

import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";
