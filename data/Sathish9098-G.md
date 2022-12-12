##
## [GAS-1] USE FUNCTION INSTEAD OF MODIFIERS . CAN SAVE MORE WHEN USING FUNCTIONS INSTEAD OF MODIFIERS.

[File: prepo-monorepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol)

        modifier onlyAllowedMsgSenders() {
        require(_allowedMsgSenders.isIncluded(msg.sender), "msg.sender not allowed");
         _;
         }

[File: prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol)

       modifier onlyCollateral() {
       require(msg.sender == address(collateral), "msg.sender != collateral");
       _;
       }

[File: prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol)

     modifier onlyCollateral() {
    require(msg.sender == address(collateral), "msg.sender != collateral");
    _;
     }

## [GAS-2]  Use assembly to check for address(0)

Saves 6 gas per instance

[File: prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol)

         81:   if (address(managerWithdrawHook) != address(0)) managerWithdrawHook.hook(msg.sender, _amount, _amount);

         71:   if (address(withdrawHook) != address(0)) {

        51:   if (address(depositHook) != address(0)) {

[File: prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol)

       68:   if (address(_mintHook) != address(0)) _mintHook.hook(msg.sender, _amount, _amount);

       93:   if (address(_redeemHook) != address(0)) {

##

## [GAS-3]  MULTIPLE ADDRESS/ID MAPPINGS CAN BE COMBINED INTO A SINGLE MAPPING OF AN ADDRESS/ID TO A STRUCT, WHERE APPROPRIATE

If both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.


[File: prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol)

        11:   mapping(address => uint256) private userToDeposits;

        12:   mapping(address => bool) private allowedHooks;

##

## [GAS-4]  globalNetDepositCap state variable should be cached with stack variable 


[File: prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol)

          function setGlobalNetDepositCap(uint256 _newGlobalNetDepositCap) external override onlyRole(SET_GLOBAL_NET_DEPOSIT_CAP_ROLE) {
          globalNetDepositCap = _newGlobalNetDepositCap;              //@AUDIT globalNetDepositCap  CACHED 
         emit GlobalNetDepositCapChange(globalNetDepositCap);     //@AUDIT globalNetDepositCap  CACHED 
         } 

##

## [GAS-5]  globalNetDepositAmount  state variable should be cached with stack variable 

[File: prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol)    


         function recordDeposit(address _sender, uint256 _amount) external override onlyAllowedHooks {
         require(_amount + globalNetDepositAmount <= globalNetDepositCap, "Global deposit cap exceeded"); //@AUDIT   globalNetDepositAmount  
        CHACHED 
         require(_amount + userToDeposits[_sender] <= userDepositCap, "User deposit cap exceeded");
        globalNetDepositAmount += _amount;     //@AUDIT   globalNetDepositAmount  CHACHED 
        userToDeposits[_sender] += _amount;
        }    

      function recordWithdrawal(uint256 _amount) external override onlyAllowedHooks {
      if (globalNetDepositAmount > _amount) { globalNetDepositAmount -= _amount; }      //@AUDIT   globalNetDepositAmount  CHACHED 
      else { globalNetDepositAmount = 0; }                  
      }
       
##

## [GAS-6]  <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES . FOR EVERY CALL CAN SAVE 13 GAS 

[File: prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol)  

       31:   globalNetDepositAmount += _amount;

       32:   userToDeposits[_sender] += _amount;

       36:   if (globalNetDepositAmount > _amount) { globalNetDepositAmount -= _amount; }

[File: prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol)

       71:   userToAmountWithdrawnThisPeriod[_sender] += _amountBeforeFee;

##

## [GAS-7]  Use uint256 instead uint24 . Possible to save 6 gas 

[File: prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol)

         12:    uint24 public constant override POOL_FEE_TIER = 10000;

##

## [GAS-8]  WE CAN USE ++X / --X  INSTEAD OF [Y]=X+1 OR [Y]=X-1 WHEN WE INCREASE OR DECREASE BY 1 . WE CAN SAVE 116 GAS 

[File: prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol)

    55:     finalLongPayout = MAX_PAYOUT + 1;

























