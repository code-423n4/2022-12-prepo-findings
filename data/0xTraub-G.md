PrePOMarket.sol

1. 
require(longToken.balanceOf(msg.sender) >= _longAmount, "Insufficient long tokens");
require(shortToken.balanceOf(msg.sender) >= _shortAmount, "Insufficient short tokens");

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L77-L78

These can both be removed since later on you call 

    longToken.burnFrom(msg.sender, _longAmount);
    shortToken.burnFrom(msg.sender, _shortAmount);

Since you can never burn more tokens than available to the user, the check is redundant and burnFrom will fail if _longAmount is > balanceOf(msg.sender) anyways. Saves gas from the two checks not needing to occur

2.     require(collateral.balanceOf(msg.sender) >= _amount, "Insufficient collateral");
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L67

Is redundant. On line 69 `collateral.transferFrom(msg.sender, address(this), _amount);`, as long as transferFrom success is properly validated and reverts on failure, you do not need to check that the their balance >= _amount, since transferFrom will fail anyways due to insufficient balance. Saves gas on the external balanceOf call and the comparison not needing to occur

--- AccountList.sol ---
1.   uint256 private resetIndex;
  mapping(uint256 => mapping(address => bool)) private _resetIndexToAccountToIncluded;
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/AccountList.sol#L8-L9

1. Can replace resetIndex with a uint16 since 2^16 = 65536 which won't ever be exceeded by ownership, or even a uint8 if you think that 256 resets is unlikely
2. Also allows you to wrap the entire `set` and `reset` function body in an `unchecked` block which saves gas since the risk of such a value overflowing is very minimal

--- DepositRecord.sol ---

1. require(_amount + globalNetDepositAmount <= globalNetDepositCap, "Global deposit cap exceeded");
    require(_amount + userToDeposits[_sender] <= userDepositCap, "User deposit cap exceeded");
    globalNetDepositAmount += _amount;
    userToDeposits[_sender] += _amount;

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L31-L32

  You already checked for maximum values in the require statements above so you can wrap these last two lines in an `unchecked` block since there's no chance of overflow since userDepositCap <= type(uint).max.

2. . function recordWithdrawal(uint256 _amount) external override onlyAllowedHooks {
    if (globalNetDepositAmount > _amount) { globalNetDepositAmount -= _amount; }
    else { globalNetDepositAmount = 0; }
  }

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L35-L38

Instead of using if-else which costs additional gas through jumps, replace with a `require(_amount <= globalNetDepositAmount)` and then in an unchecked block `unchecked {globalNetDepositAmount -= amount}` since there's no chance of an underflow because amount cannot be greater than globalNetDepositAmount



