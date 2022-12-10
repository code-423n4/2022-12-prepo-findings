##
## [GAS-1] USE FUNCTION INSTEAD OF MODIFIERS . CAN SAVE MORE WHEN USING FUNCTIONS INSTEAD OF MODIFIERS.

[File: prepo-monorepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol)

        modifier onlyAllowedMsgSenders() {
        require(_allowedMsgSenders.isIncluded(msg.sender), "msg.sender not allowed");
         _;
         }

## [GAS-2]  Use assembly to check for address(0)

Saves 6 gas per instance

[File: prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol)

         81:   if (address(managerWithdrawHook) != address(0)) managerWithdrawHook.hook(msg.sender, _amount, _amount);

         71:   if (address(withdrawHook) != address(0)) {

        51:   if (address(depositHook) != address(0)) {

        




















GAS-1	Using bools for storage incurs overhead	4
GAS-2	Use Custom Errors	37
GAS-3	Don't initialize variables with default value	3
GAS-4	Using private rather than public for constants, saves gas	40
GAS-5	Use != 0 instead of > 0 for unsigned integer comparison	15

