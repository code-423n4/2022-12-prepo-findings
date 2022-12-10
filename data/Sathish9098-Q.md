##
## [NC-1]  USE A MORE RECENT VERSION OF SOLIDITY. LATEST SOLIDITY VERSION IS 0.8.17

Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(,) Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free function

> There is 6 instance of this issue:

       pragma solidity =0.8.7;

##

## [NC-2] State variables always declare without _. The function parameters only declared with _ . But in AccountListCaller contract variables declared in reverse way. This is not a good code practice. So should follow the standard code practice .

 [File:  prepo-monorepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol)

       contract AccountListCaller is IAccountListCaller {
      IAccountList internal _accountList;             // AUDIT _

      function setAccountList(IAccountList accountList) public virtual override {           // AUDIT _
      _accountList = accountList;
    emit AccountListChange(accountList);
     }

    function getAccountList() external view override returns (IAccountList) {
    return _accountList;
     }
    }

 [File: prepo-monorepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol)

      contract AllowedMsgSenders is IAllowedMsgSenders {
     IAccountList private _allowedMsgSenders;      // AUDIT _

     modifier onlyAllowedMsgSenders() {
    require(_allowedMsgSenders.isIncluded(msg.sender), "msg.sender not allowed");
    _;
    }

    function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override {    // AUDIT _
    _allowedMsgSenders = allowedMsgSenders;
    emit AllowedMsgSendersChange(allowedMsgSenders);
    }

##

##[L-1]   MISSING CHECKS FOR ADDRESS(0X0) WHEN ASSIGNING VALUES TO ADDRESS STATE VARIABLES

[File: prepo-monorepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol)
 
      address internal _treasury;                  //@AUDIT NO ZERO ADDRESS CHECK 
      ITokenSender internal _tokenSender;

     function setTreasury(address treasury) public virtual override {
    _treasury = treasury;                    //@AUDIT NO ZERO ADDRESS CHECK 
    emit TreasuryChange(treasury);
    }

##

## [NC-3]  EXPRESSIONS FOR CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT

While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand. There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts. constants should be used for literal values written into the code, and immutable variables should be used for expressions, or values calculated in, or passed into the constructor.

[File: prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol)

     21:  bytes32 public constant MANAGER_WITHDRAW_ROLE = keccak256("Collateral_managerWithdraw(uint256)");
     22:  bytes32 public constant SET_MANAGER_ROLE = keccak256("Collateral_setManager(address)");
     23:  bytes32 public constant SET_DEPOSIT_FEE_ROLE = keccak256("Collateral_setDepositFee(uint256)");
     24:  bytes32 public constant SET_WITHDRAW_FEE_ROLE = keccak256("Collateral_setWithdrawFee(uint256)");
     25:  bytes32 public constant SET_DEPOSIT_HOOK_ROLE = keccak256("Collateral_setDepositHook(ICollateralHook)");
     26:  bytes32 public constant SET_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setWithdrawHook(ICollateralHook)");
     27:  bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");

##

##  [NC-4]  FUNCTIONS, PARAMETERS AND VARIABLES IN SNAKE CASE

  Use camel case for all functions, parameters and variables and snake case for constants

Snake case examples

       The following variable names follow the snake case naming convention:
        this_is_snake_case

Camel case examples

         The following are examples of variables that use the camel case naming convention:

          FileNotFoundException
          toString

[File: prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol)


           19:   uint256 public constant FEE_DENOMINATOR = 1000000;
           20:   uint256 public constant FEE_LIMIT = 100000;
           21:  bytes32 public constant MANAGER_WITHDRAW_ROLE = keccak256("Collateral_managerWithdraw(uint256)");
           22:  bytes32 public constant SET_MANAGER_ROLE = keccak256("Collateral_setManager(address)");
           23:  bytes32 public constant SET_DEPOSIT_FEE_ROLE = keccak256("Collateral_setDepositFee(uint256)");
           24:  bytes32 public constant SET_WITHDRAW_FEE_ROLE = keccak256("Collateral_setWithdrawFee(uint256)");
           25:  bytes32 public constant SET_DEPOSIT_HOOK_ROLE = keccak256("Collateral_setDepositHook(ICollateralHook)");
           26:  bytes32 public constant SET_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setWithdrawHook(ICollateralHook)");
           27:  bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");

##

## [NC-5]  CONSTANTS SHOULD BE DEFINED RATHER THAN USING MAGIC NUMBERS

[File: prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol)

            31:    baseTokenDenominator = 10**_newBaseTokenDecimals;


       
  

















NC-1	Return values of approve() not checked	8
L-1	abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()	1
L-2	Unsafe ERC20 operation(s)