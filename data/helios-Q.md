# 1st report for : AccountListCaller.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol

## 1. Reentrancy Attack Vulnerability in AccountListCaller contract

Summary: 
The AccountListCaller contract is vulnerable to a reentrancy attack. This could allow an attacker to repeatedly call the `setAccountList` function and potentially cause an infinite loop.

Impact: 
An attacker could exploit this vulnerability to cause an infinite loop and potentially consume all the gas available to the contract, making it unresponsive. This could lead to denial of service for the users of the contract.

Recommendation: 
To mitigate this vulnerability, the contract should use a mutex pattern to prevent reentrancy. This can be done by using a `bool` variable to track whether the contract is currently executing the `setAccountList` function, and checking this variable before calling the function again.

## 2. Unrestricted AccountListChange Emission

Summary: 
The contract has a function named `setAccountList` which emits an `AccountListChange` event. This event can be emitted by any caller without any restrictions, allowing any attacker to manipulate the state of the contract.

Impact: 
This vulnerability can be exploited by an attacker to manipulate the `_accountList` state variable, potentially leading to loss of funds or other undesired effects.

Recommendation: 
The `setAccountList` function should include some kind of restriction on who is allowed to emit the `AccountListChange` event. For example, it could include a require statement that only allows certain addresses to emit the event. This would help to prevent attackers from exploiting the vulnerability.

## 3. Uninitialized contract storage pointer

Summary:
The contract's `_accountList` storage pointer is not initialized in the contract constructor. This means that it will be initialized to `null` and any calls to `getAccountList()` will return `null`, which can lead to unexpected behavior.

Impact:
If the `_accountList` storage pointer is not initialized, any calls to `getAccountList()` will return `null`. This can cause the contract to behave in unexpected ways, potentially leading to errors or vulnerabilities.

Recommendation:
It is recommended to initialize the `_accountList` storage pointer in the contract constructor to prevent unexpected behavior and potential vulnerabilities. This can be done by adding the following line to the constructor:
```
_accountList = new IAccountList();
```

# 2nd report for : AllowedMsgSenders.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol

## 1. Lack of Access Control in AllowedMsgSenders contract

Summary: 
The `AllowedMsgSenders` contract allows any caller to set the `_allowedMsgSenders` variable through the `setAllowedMsgSenders()` function. This can allow an attacker to provide a malicious `IAccountList` contract and potentially bypass the access control restrictions within the contract.

Impact: 
If an attacker is able to successfully set a malicious `IAccountList` contract, they could potentially gain unauthorized access to sensitive information or perform unauthorized actions within the system.

Recommendation: 
Access control should be implemented in the `setAllowedMsgSenders()` function to ensure that only authorized users are able to set the `_allowedMsgSenders` variable. This could be achieved by adding a check that the caller has the necessary permissions before allowing the `_allowedMsgSenders` variable to be set.

# 3rd report for : NFTScoreRequirement.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol

## 1. Potential Integer Overflow in NFTScoreRequirement contract

Summary: 
The `NFTScoreRequirement` contract contains a potential integer overflow vulnerability in the `getAccountScore()` function. The function uses the `+=` operator to add `collectionScore` to `score` without checking for an overflow. If the sum of `collectionScore` and `score` is larger than the maximum value that can be stored in a `uint256` variable, an integer overflow will occur and the result will be incorrect.

Impact: 
If an integer overflow occurs in the `getAccountScore()` function, it could potentially lead to incorrect results being returned. This could result in users being incorrectly denied or granted access to certain features within the system.

Recommendation: 
To prevent an integer overflow from occurring, the `getAccountScore()` function should be modified to check for an overflow before adding `collectionScore` to `score`. This can be achieved by using the `SafeMath` library or by using a conditional statement to check if the sum of `collectionScore` and `score` is greater than the maximum value that can be stored in a `uint256` variable.

## 2. Lack of Access Control in NFTScoreRequirement contract

Summary: 
The `NFTScoreRequirement` contract allows any caller to set the `_requiredScore` and `_collectionToScore` variables through the `setRequiredScore()` and `setCollectionScores()` functions, respectively. This can allow an attacker to provide malicious values for these variables and potentially compromise the security of the contract.

Impact: 
If an attacker is able to successfully set malicious values for the `_requiredScore` and `_collectionToScore` variables, they could potentially bypass the access control implemented in the `_satisfiesScoreRequirement()` function and gain unauthorized access to sensitive functions within the contract.

Recommendation: 
Access control should be implemented in the `setRequiredScore()` and `setCollectionScores()` functions to ensure that only authorized users are able to set the `_requiredScore` and `_collectionToScore` variables. This could be achieved by adding a check that the caller has the necessary permissions before allowing these variables to be set. Additionally, the `onlyAllowedMsgSenders` modifier should be applied to these functions to prevent the `_requiredScore` and `_collectionToScore` variables from being set by non-authorized users.

## 3. Incorrect Check in _satisfiesScoreRequirement() Function

Summary: 
The `_satisfiesScoreRequirement()` function in the `NFTScoreRequirement` contract checks whether `_requiredScore` is equal to 0, and if so, always returns `true`. This can allow an attacker to bypass the requirement to have a minimum score by setting `_requiredScore` to 0.

Impact: 
If an attacker is able to set `_requiredScore` to 0, they could potentially bypass the requirement to have a minimum score, allowing them to gain access to sensitive functions or resources that they would not normally have access to.

Recommendation: 
The `_satisfiesScoreRequirement()` function should be modified to check whether `_requiredScore` is greater than 0, and if so, compare `getAccountScore(account)` to `_requiredScore` to determine if the minimum score requirement is satisfied. Additionally, measures should be put in place to prevent unauthorized users from setting `_requiredScore` to 0.

## 4. Unchecked Increment in NFTScoreRequirement contract

Summary: 
The `NFTScoreRequirement` contract uses an unchecked increment in the for loops in the `setCollectionScores()` and `removeCollections()` functions. This can result in an infinite loop if the `collections` parameter in either of these functions has a length of `0`.

Impact: 
An infinite loop can cause the contract to become unresponsive and potentially lead to a denial of service attack.

Recommendation: 
The `for` loops in the `setCollectionScores()` and `removeCollections()` functions should be modified to use a checked increment to prevent the possibility of an infinite loop. For example, the `for` loop in the `setCollectionScores()` function could be rewritten as follows:
```
for (uint256 i = 0; i < numCollections; i++) {
  require(scores[i] > 0, "score == 0");
  _collectionToScore.set(address(collections[i]), scores[i]);
}
```

# 4th report for : TokenSenderCaller.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol

## 1. Lack of Access Control in TokenSenderCaller contract

Summary: 
The `TokenSenderCaller` contract allows any caller to set the `_tokenSender` and `_treasury` variables through the `setTokenSender()` and `setTreasury()` functions, respectively. This can allow an attacker to provide malicious `ITokenSender` and address contracts and potentially compromise the security of the system.

Impact: 
If an attacker is able to successfully set a malicious `ITokenSender` contract, they could potentially gain unauthorized access to sensitive information or perform unauthorized actions within the system. Additionally, if an attacker is able to set a `malicious` address contract, they could potentially steal funds from the treasury.

Recommendation: 
Access control should be implemented in the `setTokenSender()` and `setTreasury()` functions to ensure that only authorized users are able to set the `_tokenSender` and `_treasury` variables. This could be achieved by adding a check that the caller has the necessary permissions before allowing the variables to be set.

## 2. Unchecked Call to External Contract in TokenSenderCaller contract

Summary: 
The `TokenSenderCaller` contract contains an unchecked call to an external contract in the `getTokenSender()` function. This can allow an attacker to provide a malicious `ITokenSender` contract that could potentially compromise the security of the system.

Impact: 
If an attacker is able to successfully set a malicious `ITokenSender` contract, they could potentially gain unauthorized access to sensitive information or perform unauthorized actions within the system.

Recommendation: 
The unchecked call to the external contract in the `getTokenSender()` function should be fixed by adding a check to ensure that the contract is not a malicious one. This could be achieved by adding a whitelist of trusted `ITokenSender` contracts, and checking that the contract being returned by the `getTokenSender()` function is on the whitelist. This will prevent malicious contracts from being returned and potentially causing harm to the system.

# 5th report for : Collateral.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol

## 1. Lack of Access Control in Collateral contract

Summary: 
The `Collateral` contract allows any caller to set the `manager`, `depositFee`, `withdrawFee`, `depositHook`, `withdrawHook`, and `managerWithdrawHook` variables through the corresponding `set*()` functions. This can allow an attacker to provide malicious values for these variables and potentially compromise the security of the system.

Impact: 
If an attacker is able to successfully set malicious values for the `manager`, `depositFee`, `withdrawFee`, `depositHook`, `withdrawHook`, and `managerWithdrawHook` variables, they could potentially gain unauthorized access to sensitive information or perform unauthorized actions within the system.

Recommendation: 
Access control should be implemented in the `set*()` functions to ensure that only authorized users are able to set the `manager`, `depositFee`, `withdrawFee`, `depositHook`, `withdrawHook`, and `managerWithdrawHook` variables. This could be achieved by adding checks that the caller has the necessary permissions before allowing the variables to be set. The `__SafeAccessControlEnumerable_grantRole()` function provided by the `SafeAccessControlEnumerableUpgradeable` contract can be used to grant access to these functions to specific users or contracts.

## 2. Reentrancy Vulnerability in Collateral contract

Summary: 
The `Collateral` contract contains a reentrancy vulnerability in the `deposit()` and `withdraw()` functions. Both functions call external contracts (`depositHook` and `withdrawHook` respectively) without using the `ReentrancyGuardUpgradeable` contract to protect against reentrancy attacks. This can allow an attacker to repeatedly call the `deposit()` or `withdraw()` functions and potentially cause unexpected behavior or loss of funds.Additionally, the `onlyAllowedMsgSenders` modifier from the `AllowedMsgSenders` contract (if available) could be applied to these functions to prevent unauthorized users from setting the variables.

Impact: 
If an attacker is able to successfully exploit the reentrancy vulnerability in the `Collateral` contract, they could potentially cause unexpected behavior or loss of funds in the contract.

Recommendation: 
The reentrancy vulnerability in the `deposit()` and `withdraw()` functions should be fixed by using the `ReentrancyGuardUpgradeable` contract to protect against reentrancy attacks. This can be done by calling the `__reentrancyGuard_beforeHook()` function before calling the `depositHook` or `withdrawHook` contracts, and the `__reentrancyGuard_afterHook()` function after calling these contracts.

## 3. Unbounded Loop in Collateral contract

Summary: 
The `Collateral` contract contains an unbounded loop in the `setDepositFee()` and `setWithdrawFee()` functions. The functions use a `for` loop with the `unchecked` keyword, which can potentially cause the loop to run indefinitely and result in a contract crash.

Impact: 
If an attacker is able to successfully exploit the unbounded loop vulnerability, they could potentially cause the contract to crash, resulting in loss of funds and disruption of service.

Recommendation: 
The unbounded loop vulnerability in the `setDepositFee()` and `setWithdrawFee()` functions should be fixed by replacing the `for` loop with a `while` loop that has a defined end condition. This will ensure that the loop will not run indefinitely, even if the `unchecked` keyword is used.

## 4. Integer Overflow in Collateral contract

Summary: 
The `Collateral` contract contains an integer overflow vulnerability in the `withdraw()` function. The function calculates the `_fee` variable by multiplying `_amount` by `withdrawFee` and dividing by `FEE_DENOMINATOR` without checking for overflow, which can allow an attacker to manipulate the `_fee` variable and potentially cause unexpected behavior.

Impact: 
If an attacker is able to successfully exploit the integer overflow vulnerability, they could potentially cause unexpected behavior in the contract, such as incorrect results or contract failure.

Recommendation: 
The integer overflow vulnerability in the `withdraw()` function should be fixed by checking for overflow before calculating the `_fee` variable. This could be achieved by using the `SafeMath` library provided by `OpenZeppelin` to perform arithmetic operations on the `_fee` variable in a way that prevents overflow.
```
import "@openzeppelin/contracts/math/SafeMath.sol";

contract Collateral is ICollateral, ERC20PermitUpgradeable, SafeAccessControlEnumerableUpgradeable, ReentrancyGuardUpgradeable {
  ...

  function withdraw(address _recipient, uint256 _amount) external override nonReentrant returns (uint256) {
    ...
    uint256 _fee = (SafeMath.mul(_amount, withdrawFee)) / FEE_DENOMINATOR;
    ...
```

# 6th report for : DepositHook.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol

## 1. Lack of input validation in `hook()` function

Summary: 
The `hook()` function in the `DepositHook` contract does not adequately validate input parameters. This can lead to incorrect processing of deposit records and fee payments.

Impact: 
If an attacker is able to provide malicious input to the `hook()` function, they may be able to cause the contract to record incorrect deposit amounts and make incorrect payments from the collateral contract. This could result in financial losses for the contract owner and users.

Recommendation: 
The `hook()` function should validate the input parameters to ensure they are within the expected range and have the correct format. In particular, the `_amountBeforeFee` and `_amountAfterFee` parameters should be checked to ensure they are non-zero and that `_amountBeforeFee` is greater than or equal to `_amountAfterFee`. This will prevent attackers from providing malicious input that could cause the contract to malfunction.

## 2.  Incorrect modifier usage in hook function

Summary: 
The `hook` function in the `DepositHook` contract incorrectly uses the `onlyCollateral` modifier on the `override` keyword instead of the function definition. This allows anyone to call the `hook` function and bypass the intended access control.

Impact: 
This vulnerability allows anyone to call the `hook` function, potentially allowing them to manipulate the deposit record and send funds to the treasury without satisfying the required score requirement or the deposits being allowed. This could result in incorrect recording of deposits and unauthorized transfer of funds.

Recommendation: 
The `onlyCollateral` modifier should be applied to the `hook` function definition instead of the `override` keyword. The correct usage would be:
```
function hook(address _sender, uint256 _amountBeforeFee, uint256 _amountAfterFee) external override onlyCollateral {
  ...
}
```

## 3.  Incorrect check for whether deposits are allowed

Summary: 
The `hook` function includes a check to verify that `depositsAllowed` is true before proceeding, but this variable is never initialized or set to any value. As a result, the `hook` function will always revert with the error message "deposits not allowed" when called.

Impact: 
Any contract that calls the `hook` function will be unable to deposit funds, preventing the intended functionality of the contract.

Recommendation: 
Initialize the `depositsAllowed` variable to `false` or `true` as appropriate, or remove the check for whether deposits are allowed if this variable is not intended to be used. Alternatively, set the value of `depositsAllowed` using the `setDepositsAllowed` function before calling `hook`.

## 4. TransferFrom Call in hook() Can Lead to Unauthorized Transfer
Summary
In the `hook()` function, the contract uses the `transferFrom()` function to transfer a fee amount to the `_treasury` address. However, this function call is susceptible to a reentrancy attack that can allow an attacker to perform unauthorized transfers.

Impact
An attacker can exploit this vulnerability to perform unauthorized transfers of the underlying token used by the contract. This can result in loss of funds for the contract or its users.

Recommendation
To prevent this vulnerability, the `hook()` function should use a `transfer()` function call instead of `transferFrom()`. This will ensure that the function cannot be called again in the middle of the transfer, and therefore cannot be exploited for unauthorized transfers. Additionally, the contract should implement a reentrancy guard to further protect against this type of attack.

## 5. OnlyCollateral Modifier Bypass

Summary: 
The `onlyCollateral()` modifier is being used to restrict access to the `hook()` function. However, the `msg.sender` value is being compared to the `collateral` contract instead of the `_sender` parameter. Because the `msg.sender` value cannot be changed by the caller of the `hook()` function, this check will always pass, allowing anyone to call the `hook()` function.

Impact: 
This vulnerability allows anyone to call the `hook()` function, potentially allowing them to manipulate the deposit record, transfer fees from the collateral contract, and send tokens to themselves.

Recommendation: 
The check in the `onlyCollateral()` modifier should be changed to compare the `msg.sender` value to the `_sender` parameter instead of the `collateral` contract. This will ensure that only the caller of the `hook()` function can access the function.

# 7th report for : DepositRecord.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol

## 1. User deposit cap not checked in `recordWithdrawal`

Summary: 
The `recordWithdrawal` function does not check if the user's deposit cap will be exceeded when performing a withdrawal.

Impact: 
This can allow a user to withdraw more than their deposit cap, which might result in incorrect balances or other issues.

Recommendation: 
Add a check to `recordWithdrawal` to ensure that the user's deposit cap is not exceeded when performing a withdrawal. This can be done by adding a line of code similar to the following:
```
require(_amount <= userToDeposits[_sender], "User deposit cap exceeded");
```

## 2. Insecure default value for `allowedHooks` mapping

Summary: 
The `allowedHooks` mapping is declared with no default value, allowing arbitrary contracts to call the `onlyAllowedHooks` modifier and potentially modify the `globalNetDepositAmount` and `userToDeposits` state variables.

Impact: 
An attacker could exploit this vulnerability to manipulate the `globalNetDepositAmount` and `userToDeposits` state variables without any authorization checks, potentially leading to funds being stolen or lost.

Recommendation: 
Set a default value of false for the `allowedHooks` mapping. This can be done by declaring the mapping as `mapping(address => bool) private allowedHooks = false;`. Alternatively, a `require` statement can be added at the beginning of the `onlyAllowedHooks` modifier to ensure that only authorized contracts can call the modifier.

## 3. Incorrect type of `globalNetDepositAmount` variable

Summary: 
The `globalNetDepositAmount` variable is declared as type `uint256`, but it should be declared as type `uint128` to avoid potential integer overflow issues.

Impact: 
If the `globalNetDepositAmount` variable overflows, it can result in incorrect calculations and unexpected behavior.

Recommendation: 
Update the declaration of `globalNetDepositAmount` to use the `uint128` type to avoid potential integer overflow issues.

## 4. Incorrect handling of edge case in function `recordWithdrawal`

Summary: 
The function `recordWithdrawal` does not handle the edge case where `_amount` is greater than `globalNetDepositAmount`. In this case, `globalNetDepositAmount` should be set to `0` to avoid an underflow.

Impact: 
If `_amount` is greater than `globalNetDepositAmount`, the value of `globalNetDepositAmount` will be set to a negative value, which can cause issues in other parts of the code that depend on its value being non-negative.

Recommendation: 
Update the `recordWithdrawal` function to handle the edge case where `_amount` is greater than `globalNetDepositAmount` by setting `globalNetDepositAmount` to `0` in this case.

## 5. Incorrect return type for function `getUserDepositAmount`

Summary: 
The function `getUserDepositAmount` has an incorrect return type. It should be `function getUserDepositAmount(address) external view returns (uint256)`.

Impact: 
The incorrect return type of `getUserDepositAmount` might cause issues when calling this function as the expected return value is not defined.

Recommendation: 
Update the return type of `getUserDepositAmount` to `function getUserDepositAmount(address) external view returns (uint256)` to avoid potential issues.

## 6. Integer Overflow/Underflow in `recordDeposit` function

Summary: 
The `recordDeposit` function in the `DepositRecord` contract contains two checks that may cause integer overflow/underflow when adding `_amount` to `globalNetDepositAmount` and `userToDeposits[_sender]`.

Impact: 
If the `_amount` parameter is large enough, the addition operations in the `recordDeposit` function will overflow/underflow, leading to incorrect values being stored in the `globalNetDepositAmount` and `userToDeposits[_sender]` variables. This could result in incorrect balances being recorded, leading to unexpected behavior in other parts of the contract and potentially allowing malicious actors to exploit the contract.

Recommendation: 
Add the `SafeMath` library to the contract and use its functions for the addition operations in the `recordDeposit` function to prevent integer overflow/underflow. For example, replace `_amount + globalNetDepositAmount` with `SafeMath.add(_amount, globalNetDepositAmount)`.

# 8th report for : DepositTradeHelper.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol

## 1. Depositing and trading on behalf of others

Summary: 
The `depositAndTrade` function allows the contract owner to deposit and trade on behalf of other users.

Impact: 
This can be abused by the contract owner to manipulate the market, potentially leading to losses for other users.

Recommendation: 
Add checks to ensure that the `msg.sender` is the same as the user for whom the deposit and trade is being performed. This can be done by adding a requirement that the `msg.sender` is the same as the `baseTokenPermit.holder` and the `collateralPermit.holder` in the `depositAndTrade` function. For example:
```
function depositAndTrade(uint256 baseTokenAmount, Permit calldata baseTokenPermit, Permit calldata collateralPermit, OffChainTradeParams calldata tradeParams) external override {
  require(msg.sender == baseTokenPermit.holder, "msg.sender != baseTokenPermit.holder");
  require(msg.sender == collateralPermit.holder, "msg.sender != collateralPermit.holder");

  if (baseTokenPermit.deadline != 0) {
    IERC20Permit(address(_baseToken)).permit(msg.sender, address(this), type(uint256).max, baseTokenPermit.deadline, baseTokenPermit.v, baseTokenPermit.r, baseTokenPermit.s);
  }
  _baseToken.transferFrom(msg.sender, address(this), baseTokenAmount);
  if (collateralPermit.deadline != 0) {
    _collateral.permit(msg.sender, address(this), type(uint256).max, collateralPermit.deadline, collateralPermit.v, collateralPermit.r, collateralPermit.s);
  }
  uint256 _collateralAmountMinted = _collateral.deposit(msg.sender, baseTokenAmount);
  _collateral.transferFrom(msg.sender, address(this), _collateralAmountMinted);
  ISwapRouter.ExactInputSingleParams memory exactInputSingleParams = ISwapRouter.ExactInputSingleParams(address(_collateral), tradeParams.tokenOut, POOL_FEE_TIER, msg.sender, tradeParams.deadline, _collateralAmountMinted, tradeParams.amountOutMinimum, tradeParams.sqrtPriceLimitX96);
  _swapRouter.exactInputSingle(exactInputSingleParams);
}
```

## 2. Lack of user input validation in `depositAndTrade` function

Summary: 
The `depositAndTrade` function does not validate user input for the `baseTokenAmount`, `baseTokenPermit`, `collateralPermit`, and `tradeParams` parameters.

Impact: 
This can allow an attacker to supply malicious input to the `depositAndTrade` function, potentially leading to the transfer of excessive funds or other unintended behavior.

Recommendation: 
Add input validation to the `depositAndTrade` function to ensure that the supplied values are within the expected range and format. For example, the `baseTokenAmount` and `tradeParams.amountOutMinimum` parameters should be checked to ensure they are greater than zero, and the `baseTokenPermit` and `collateralPermit` parameters should be checked to ensure that the `v`, `r`, and `s` values are not zero. The `tradeParams.deadline` parameter should also be checked to ensure it is in the future. This will help to prevent potential attacks and ensure the correct functioning of the contract.

## 3. Incorrect use of `type(uint256).max`

Summary: 
The `type(uint256).max` value is used in two calls to `approve` and `permit` functions, but this value should be the maximum allowed amount for the token rather than the maximum value for a uint256 type.

Impact: 
Using `type(uint256).max` in these calls will result in an incorrect maximum allowance/permission being set, which can potentially lead to loss of funds.

Recommendation: 
Replace the use of `type(uint256).max` with the appropriate maximum allowed amount for the token in the `approve` and `permit` function calls. For example, if the token has 18 decimals, the maximum allowed amount would be `2**256 - 1` or `115792089237316195423570985008687907853269984665640564039457584007913129639935`. This value can be calculated as `(2**256 - 1) / 10**18` in Solidity.

## 4. Unauthorized ERC20 permit

Summary: 
The `_baseToken.transferFrom` function is called on the `depositAndTrade` function without checking for the `msg.sender`'s authorization.

Impact: 
This allows any caller to transfer base tokens from the `msg.sender` without their authorization, potentially leading to the loss of funds.

Recommendation: 
Add a check to ensure that `msg.sender` is authorized to transfer the base tokens before calling `_baseToken.transferFrom`. This can be done using the `_baseToken.allowance` function. For example:
```
function depositAndTrade(uint256 baseTokenAmount, Permit calldata baseTokenPermit, Permit calldata collateralPermit, OffChainTradeParams calldata tradeParams) external override {
  require(_baseToken.allowance(msg.sender, address(this)) >= baseTokenAmount, "msg.sender not authorized to transfer base tokens");
  if (baseTokenPermit.deadline != 0) {
    IERC20Permit(address(_baseToken)).permit(msg.sender, address(this), type(uint256).max, baseTokenPermit.deadline, baseTokenPermit.v, baseTokenPermit.r, baseTokenPermit.s);
  }
  _baseToken.transferFrom(msg.sender, address(this), baseTokenAmount);
  ...
}
```

## 5. Incorrect use of Permit struct in `depositAndTrade` function

Summary: 
The `baseTokenPermit` and `collateralPermit` parameters of the `depositAndTrade` function are of type `Permit`, but this type is not defined anywhere in the contract.

Impact: 
As a result, calling the `depositAndTrade` function with `Permit` arguments will fail as the contract does not know the expected format of these arguments.

Recommendation: 
Define the `Permit` struct in the contract and use it as the type for the `baseTokenPermit` and `collateralPermit` parameters in the `depositAndTrade` function. The struct should have the following format:

```
struct Permit {
  address holder;
  address spender;
  uint256 nonce;
  uint256 deadline;
  uint8 v;
  bytes32 r;
  bytes32 s;
}
```
The `depositAndTrade` function should then be updated to use this struct as follows:
```
function depositAndTrade(uint256 baseTokenAmount, Permit calldata baseTokenPermit, Permit calldata collateralPermit, OffChainTradeParams calldata tradeParams) external override {
  if (baseTokenPermit.deadline != 0) {
    IERC20Permit(address(_baseToken)).permit(baseTokenPermit.holder, baseTokenPermit.spender, baseTokenPermit.nonce, baseTokenPermit.deadline, baseTokenPermit.v, baseTokenPermit.r, baseTokenPermit.s);
  }
  _baseToken.transferFrom(msg.sender, address(this), baseTokenAmount);
  if (collateralPermit.deadline != 0) {
    _collateral.permit(collateralPermit.holder, collateralPermit.spender, collateralPermit.nonce, collateralPermit.deadline, collateralPermit.v, collateralPermit.r, collateralPermit.s);
  }
  uint256 _collateralAmountMinted = _collateral.deposit(msg.sender, baseTokenAmount);
  _collateral.transferFrom(msg.sender, address(this), _collateralAmountMinted);
  ISwapRouter.ExactInputSingleParams memory exactInputSingleParams = ISwapRouter.ExactInputSingleParams(address(_collateral), tradeParams.tokenOut, POOL_FEE_TIER, msg.sender, tradeParams.deadline, _collateralAmountMinted, tradeParams.amountOutMinimum, tradeParams.sqrtPriceLimitX96);
  _swapRouter.exactInputSingle(exactInputSingleParams);
}
``function depositAndTrade(uint256 baseTokenAmount, Permit calldata baseTokenPermit, Permit calldata collateralPermit, OffChainTradeParams calldata tradeParams) external override {
  if (baseTokenPermit.deadline != 0) {
    IERC20Permit(address(_baseToken)).permit(baseTokenPermit.holder, baseTokenPermit.spender, baseTokenPermit.nonce, baseTokenPermit.deadline, baseTokenPermit.v, baseTokenPermit.r, baseTokenPermit.s);
  }
  _baseToken.transferFrom(msg.sender, address(this), baseTokenAmount);
  if (collateralPermit.deadline != 0) {
    _collateral.permit(collateralPermit.holder, collateralPermit.spender, collateralPermit.nonce, collateralPermit.deadline, collateralPermit.v, collateralPermit.r, collateralPermit.s);
  }
  uint256 _collateralAmountMinted = _collateral.deposit(msg.sender, baseTokenAmount);
  _collateral.transferFrom(msg.sender, address(this), _collateralAmountMinted);
  ISwapRouter.ExactInputSingleParams memory exactInputSingleParams = ISwapRouter.ExactInputSingleParams(address(_collateral), tradeParams.tokenOut, POOL_FEE_TIER, msg.sender, tradeParams.deadline, _collateralAmountMinted, tradeParams.amountOutMinimum, tradeParams.sqrtPriceLimitX96);
  _swapRouter.exactInputSingle(exactInputSingleParams);
}
```

## 6. Incorrect approval of maximum token transfer

Summary: 
In the `depositAndTrade` function, the `_collateral.approve` call grants approval to the `_swapRouter` to transfer the maximum amount of `_collateral`, represented by `type(uint256).max`. However, the `_swapRouter` contract does not have a corresponding `transferFrom` call that allows it to transfer the maximum amount of `_collateral` from the `msg.sender`.

Impact: 
This could potentially allow the `_swapRouter` contract to transfer more `_collateral` than the user intended to transfer, leading to potential loss of funds.

Recommendation: 
Update the `depositAndTrade` function to only grant approval to the `_swapRouter` contract to transfer the amount of `_collateral` that the user intends to transfer. This can be done by replacing the `type(uint256).max` with the amount of `_collateral` that the user intends to transfer. For example:
```
function depositAndTrade(uint256 baseTokenAmount, Permit calldata baseTokenPermit, Permit calldata collateralPermit, OffChainTradeParams calldata tradeParams) external override {
  if (baseTokenPermit.deadline != 0) {
    IERC20Permit(address(_baseToken)).permit(msg.sender, address(this), type(uint256).max, baseTokenPermit.deadline, baseTokenPermit.v, baseTokenPermit.r, baseTokenPermit.s);
  }
  _baseToken.transferFrom(msg.sender, address(this), baseTokenAmount);
  if (collateralPermit.deadline != 0) {
    _collateral.permit(msg.sender, address(this), type(uint256).max, collateralPermit.deadline, collateralPermit.v, collateralPermit.r, collateralPermit.s);
  }
  uint256 _collateralAmountMinted = _collateral.deposit(msg.sender, baseTokenAmount);
  _collateral.approve(address(_swapRouter), _collateralAmountMinted);
  _collateral.transferFrom(msg.sender, address(this), _collateralAmountMinted);
  ISwapRouter.ExactInputSingleParams memory exactInputSingleParams = ISwapRouter.ExactInputSingleParams(address(_collateral), tradeParams.tokenOut, POOL_FEE_TIER, msg.sender, tradeParams.deadline, _collateralAmountMinted, tradeParams.amountOutMinimum, tradeParams.sqrtPriceLimitX96);
  _swapRouter.exactInputSingle(exactInputSingleParams);
}
```

# 9th report for : LongShortToken.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol

## 1. Reentrancy vulnerability in mint function

Summary: 
The `mint` function is not protected against reentrancy attacks.

Impact: 
An attacker could call the `mint` function with a malicious contract as the recipient, which could then call the `mint` function again, potentially causing the contract to `mint` an unlimited amount of tokens. This can lead to the loss of funds and can potentially be exploited to achieve other malicious goals.

Recommendation: 
Protect the `mint` function against reentrancy attacks by using the `ReentrancyGuard` contract from the OpenZeppelin library. For example:
```
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

...

contract LongShortToken is ERC20Burnable, Ownable {
  constructor(string memory name_, string memory symbol_) ERC20(name_, symbol_) {}

  using ReentrancyGuard for ReentrancyGuard.ReentrancyGuard;

  function mint(address _recipient, uint256 _amount) external onlyOwner {
    ReentrancyGuard.reentrancyGuard(block.number);
    _mint(_recipient, _amount);
  }
}
```

# 10th report for : ManagerWithdrawHook.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

## 1. Insecure use of `address` type

Summary: 
The contract defines a number of constants representing the keccak256 hashes of strings, and assigns them to `address` type variables. This is incorrect, as `address` type variables in Solidity are intended to represent Ethereum addresses, not arbitrary byte sequences.

Impact: 
This issue can cause problems when the contract tries to compare the incorrect `address` type values to other values, such as in the `onlyRole()` function modifier. This could potentially lead to security vulnerabilities, such as allowing unauthorized access to contract functions.

Recommendation: 
The variables should be defined as `bytes32` type instead of `address` type. This will ensure that they can be used to store the keccak256 hashes correctly and avoid any potential security issues.

## 2. Insufficient Minimum Reserve Percentage

Summary: 
The `ManagerWithdrawHook` contract's `hook` function does not properly enforce the minimum reserve percentage. It only checks if the collateral's reserve minus the amount of the withdrawal is greater than or equal to the minimum reserve, but it does not take into account the deposit record's global net deposit amount. As a result, the contract may allow a withdrawal that causes the reserve to fall below the minimum reserve percentage.

Impact: 
If an attacker is able to cause the contract to allow a withdrawal that falls below the minimum reserve percentage, they may be able to manipulate the contract's collateral value. This could potentially result in financial losses for users of the contract.

Recommendation: 
The `hook` function should be modified to take the global net deposit amount into account when checking if the minimum reserve percentage is being met. This can be done by modifying the `getMinReserve` function to return the product of the global net deposit amount and the minimum reserve percentage, divided by the PERCENT_DENOMINATOR constant. This will ensure that the minimum reserve percentage is properly enforced.

## 3.  Collateral's reserve is not protected against negative values

Summary:
In the `ManagerWithdrawHook` contract's `hook` function, the minimum reserve requirement is checked using the comparison `collateral.getReserve() - _amountAfterFee >= getMinReserve()`. However, if the result of `collateral.getReserve() - _amountAfterFee` is negative, the comparison will always return `true`, allowing for the reserve to be set to a negative value.

Impact:
This vulnerability allows the owner of the `ManagerWithdrawHook` contract to set the `collateral` contract's reserve to a negative value, potentially leading to a loss of funds.

Recommendation:
In the `ManagerWithdrawHook` contract's `hook` function, the minimum reserve requirement should be checked using the comparison `collateral.getReserve() >= getMinReserve() + _amountAfterFee`. This ensures that the reserve is not set to a negative value, regardless of the value of `_amountAfterFee`.

# 11th report for : MintHook.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol

## 1.  Incorrect Use of AllowedMsgSenders Contract

Summary:
The `MintHook` contract extends the `AllowedMsgSenders` contract, which provides a `onlyAllowedMsgSenders` modifier to allow only specific addresses to call contract functions. However, the `hook` function of `MintHook` incorrectly uses this modifier to check if the caller is included in an `IAccountList` contract, rather than checking if the caller is in the list of allowed addresses. This allows any address that is included in the `IAccountList` contract to call the `hook` function, regardless of whether they are actually on the allowlist.

Impact:
This bug allows any address that is included in the `IAccountList` contract to call the hook function, potentially allowing unauthorized parties to mint PrePOMarket positions.

Recommendation:
The `hook` function should check if the caller is on the allowlist of allowed addresses, rather than checking if they are included in the `IAccountList` contract. This can be achieved by replacing the line `require(_accountList.isIncluded(sender), "minter not allowed");` with `require(allowedMsgSenders.isIncluded(msg.sender), "minter not allowed");`.

# 12th report for : PrePOMarket.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol

## 1. Incorrect Ownership

Summary: 
In the `PrePOMarket` contract, the ownership of the `longToken` and `shortToken` are transferred to the contract at construction time. However, the `transferOwnership` function only updates the `owner` variable in the `Ownable` contract, and does not update the ownership of the `longToken` and `shortToken` contracts.

Impact: 
This vulnerability allows the contract owner to transfer ownership of the `PrePOMarket` contract to another address, but the `longToken` and `shortToken` contracts will still be owned by the original contract owner. This could allow the original contract owner to manipulate the `longToken` and `shortToken` contracts without the new contract owner's knowledge or consent.

Recommendation: 
The `PrePOMarket` contract should use the `ERC20.transferOwnership` function to transfer ownership of the `longToken` and `shortToken` contracts to the `PrePOMarket` contract. This function updates the ownership of the `ERC20` contract as well as the `Ownable` contract. Alternatively, the `PrePOMarket` contract could directly call the `Ownable.transferOwnership` function in the `longToken` and `shortToken` contracts to transfer ownership to the `PrePOMarket` contract.

## 2. Reentrancy vulnerability in `PrePOMarket.redeem()`

Summary:
The `redeem()` function in the `PrePOMarket` contract is vulnerable to reentrancy attacks, which could allow an attacker to repeatedly call the `redeem()` function and steal funds from the contract.

Impact:
An attacker could use this vulnerability to steal funds from the contract by calling the `redeem()` function in a loop and repeatedly withdrawing funds. This could result in significant financial losses for the contract owner and other users of the contract.

Recommendation:
To fix this issue, the `PrePOMarket` contract should be modified to use a `ReentrancyGuard` to prevent reentrancy attacks. This can be done by adding a `ReentrancyGuard` contract as a base contract, and using the `nonReentrant` modifier on the `redeem()` function to prevent reentrancy attacks.

# 13th report for : PrePOMarketFactory.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol

## 1. Lack of input validation

Summary: 
The `createMarket()` function of the `PrePOMarketFactory` contract does not properly validate its input parameters, allowing an attacker to create a market with invalid parameters.

Impact: 
An attacker can create a market with invalid parameters, which could cause issues when using the market.

Recommendation: 
The `createMarket()` function should properly validate its input parameters to ensure that the created market has valid parameters. This could include checks such as ensuring that the `floorLongPrice` is less than the `ceilingLongPrice`, and that the `expiryTime` is in the future.

## 2. Invalid Collateral Check Vulnerability

Summary:
The `createMarket()` function in the `PrePOMarketFactory` contract allows the caller to create a new `PrePOMarket` instance, which requires a valid collateral. However, the function does not properly check if the specified collateral is valid, as the `validCollateral` mapping is never updated. As a result, any address can be used as collateral for creating a new `PrePOMarket` instance.

Impact:
This vulnerability allows any address to be used as collateral for creating a new `PrePOMarket` instance, even if it is not a valid collateral. This can lead to loss of funds and potential security issues for the contract.

Recommendation:
The `createMarket()` function should properly check if the specified collateral is valid by checking if it exists in the `validCollateral` mapping. This can be done by adding a check before creating a new `PrePOMarket` instance, as shown below:
```
function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
  require(validCollateral[_collateral], "Invalid collateral");

  // Rest of the function...
}
```
## 3. Unrestricted Ownership Transfer

Summary: 
The `PrePOMarketFactory` contract allows unrestricted ownership transfer, allowing any address to call the `transferOwnership()` function and take ownership of the contract. This can be exploited to gain full control of the contract, including the ability to add new markets and modify the valid collateral list.

Impact: 
Unrestricted ownership transfer can allow attackers to gain full control of the contract, including the ability to add new markets and modify the valid collateral list. This can potentially be exploited to create malicious markets or manipulate the collateral list to steal funds from unsuspecting users.

Recommendation: 
Restrict access to the `transferOwnership()` function to only authorized addresses, such as the contract's current owner or a designated governance contract. This will prevent unauthorized ownership transfer and reduce the risk of exploitation.

## 4. Reentrancy Vulnerability in PrePOMarketFactory

Summary:
The contract `PrePOMarketFactory` is vulnerable to a reentrancy attack. The `createMarket()` function calls the `new` operator to create a new `PrePOMarket` contract, but it does not protect against reentrancy by calling `selfdestruct()` or using the `RentrancyGuard` contract. This means that an attacker can call the `createMarket()` function and then immediately call the `PrePOMarke`t contract's functions, potentially causing the contract to reenter the `createMarket()` function and creating multiple instances of the `PrePOMarket` contract.

Impact:
If an attacker is able to successfully exploit this vulnerability, they could potentially create multiple instances of the `PrePOMarket` contract. This could result in the attacker being able to mint multiple tokens, or cause other unexpected behavior in the contract.

Recommendation:
To fix this vulnerability, the `createMarket()` function should be modified to protect against reentrancy by either calling `selfdestruct()` after creating the new `PrePOMarket` contract, or by using the `ReentrancyGuard` contract to protect the `createMarket()` function.

## 5. Integer Overflow in `PrePOMarketFactory.createMarket()`

Summary: 
The `PrePOMarketFactory.createMarket()` function contains an integer overflow vulnerability in the `_createPairTokens()` function. This function creates two new `LongShortToken` contracts and assigns them to the `_newLongToken` and `_newShortToken` variables. However, these variables are declared as `LongShortToken` instead of `ILongShortToken`, which means that the `_newLongToken` and `_newShortToken` variables will not be type-checked by the `PrePOMarket.sol` contract when they are passed as arguments to the `PrePOMarket` constructor.

Impact: 
An attacker could create a malicious `LongShortToken` contract that manipulates the `_newLongToken` and `_newShortToken` variables, causing them to exceed the maximum value of a 256-bit integer. This could result in unexpected behavior and potentially allow the attacker to take advantage of the bug to gain unauthorized access or privileges.

Recommendation: 
The `_newLongToken` and `_newShortToken` variables should be declared as `ILongShortToken` instead of `LongShortToken` to ensure that they are type-checked by the `PrePOMarket.sol` contract. Additionally, the `PrePOMarketFactory.createMarket()` function should include checks to prevent integer overflows in the arguments passed to the `PrePOMarket` constructor.

# 14th report for : RedeemHook.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol

## 1.  Incorrect handling of failed transfer in RedeemHook contract

Summary: 
The `RedeemHook` contract contains a vulnerability in the `hook()` function that can result in a loss of funds. When a market has ended and users can directly settle their positions with the market contract, a fee may be taken and reimbursed using the `_tokenSender` contract. However, the `hook()` function does not check the return value of `IPrePOMarket(msg.sender).getCollateral().transferFrom()`, which could fail if the sender does not have sufficient funds or if the transfer is otherwise unable to be completed. If this function fails, the fee will not be transferred to the treasury and the `_tokenSender.send()` function will be called with a non-zero fee value, leading to a loss of funds for the contract.

Impact: 
If this vulnerability is exploited, it could result in a loss of funds for the contract. This could potentially impact the functionality of the contract and result in financial losses for users.

Recommendation: 
To fix this vulnerability, the `hook()` function should check the return value of `IPrePOMarket(msg.sender).getCollateral().transferFrom()` and only call `_tokenSender.send()` if the transfer was successful.

## 2. Misuse of msg.sender in RedeemHook contract

Summary: 
The `RedeemHook` contract uses `msg.sender` to obtain the address of the contract calling the `hook` function, but this can be easily manipulated by an attacker.

Impact: 
This vulnerability allows an attacker to manipulate the `hook` function in order to bypass the `onlyAllowedMsgSenders` modifier and the `_accountList.isIncluded` check, potentially allowing unauthorized users to redeem positions and collect fees.

Recommendation: 
Replace the use of `msg.sender` with the `sender` parameter passed to the `hook` function. This will ensure that the `hook` function only processes redemption requests from the intended sender.

## 3. Missing check on `_treasury` in `hook()` function

Summary: 
The `hook()` function in contract `RedeemHook` allows a user to transfer fees to the `_treasury` account without checking that the `_treasury` account is not the zero address. This can result in the fees being transferred to the zero address, which can lead to the loss of funds.

Impact: 
The impact of this vulnerability is that funds can be lost if the `_treasury` address is not set or is set to the zero address.

Recommendation: 
To fix this vulnerability, the `hook()` function should check that the `_treasury` address is not the zero address before transferring fees to it. This can be done by adding the following line before the `IPrePOMarket(msg.sender).getCollateral().transferFrom(msg.sender, _treasury, fee);` line:
```
require(_treasury != address(0), "treasury address is not set");
```
## 4. Incorrect implementation of `hook` function

Summary: 
In the `hook` function of the `RedeemHook` contract, the `msg.sender` is not used correctly. The `_accountList.isIncluded` method is called with `sender` as an argument, but it should be called with `msg.sender`.

Impact: 
This bug can allow any address to call the `hook` function, even if they are not in the `_accountList` of allowed users. This can result in incorrect fees being calculated and transferred to the `_treasury` address.

Recommendation: 
To fix this bug, the `_accountList.isIncluded` method should be called with `msg.sender` instead of `sender`. This will ensure that only addresses that are included in the `_accountList` are allowed to call the `hook` function.

# 15th report for : TokenSender.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol

## 1. Incorrect price calculation in the `send` function

Summary: 
The `send` function in the `TokenSender` contract calculates the scaled price by multiplying the price with the price multiplier, then dividing it by `MULTIPLIER_DENOMINATOR` (10000). However, this calculation is incorrect because it should be doing integer division, not floating-point division. As a result, the calculated price may be off by a factor of up to 10000.

Impact: 
If the calculated price is off by a factor of 10000, then the amount of tokens that are transferred to the recipient may be significantly larger or smaller than intended. This could potentially cause financial losses for the users of the contract.

Recommendation: 
To fix this issue, the `send` function should use integer division instead of floating-point division. This can be done by changing the division operator (/) to the integer division operator (`div`) in the `send` function. Here is the updated code:
```
function send(address recipient, uint256 unconvertedAmount) external override onlyAllowedMsgSenders {
  uint256 scaledPrice = (_price.get() * _priceMultiplier) div MULTIPLIER_DENOMINATOR;
  if (scaledPrice <= _scaledPriceLowerBound) return;
  uint256 outputAmount = (unconvertedAmount * _outputTokenDecimalsFactor) / scaledPrice;
  if (outputAmount == 0) return;
  if (outputAmount > _outputToken.balanceOf(address(this))) return;
  _outputToken.transfer(recipient, outputAmount);
}
```

## 2. Reentrancy vulnerability in the `send()` function

Summary: 
The `send()` function of the `TokenSender` contract is vulnerable to a reentrancy attack. Specifically, the `transfer()` function call to the `_outputToken` contract may execute arbitrary code that could call back into the `TokenSender` contract and cause the `send()` function to be executed again. This could allow an attacker to manipulate the state of the contract and potentially steal tokens.

Impact: 
If an attacker is able to exploit this vulnerability, they could manipulate the state of the `TokenSender` contract and potentially steal tokens.

Recommendation: 
To fix this vulnerability, the `send()` function should be refactored to use the `safeTransfer()` function from the OpenZeppelin `SafeERC20` contract, which prevents reentrancy attacks by disabling the caller's code during the transfer. This can be done by adding the following import statement to the contract:
```
import "@openzeppelin/contracts/token/ERC20/SafeERC20.sol";
```
and then replacing the `_outputToken.transfer(recipient, outputAmount)` line with the following:
```
SafeERC20(_outputToken).safeTransfer(recipient, outputAmount);
```

## 3. SafeAccessControlEnumerable not imported

Summary: 
The contract `TokenSender` imports the contract `SafeAccessControlEnumerable` but does not use it. This results in unnecessary gas usage and potential security vulnerabilities.

Impact: 
The contract is unnecessarily gas-inefficient, and there may be potential security vulnerabilities if the imported contract has any vulnerabilities.

Recommendation: 
Remove the import statement for `SafeAccessControlEnumerable` and any references to it in the contract. If the contract is intended to inherit from `SafeAccessControlEnumerable`, the import statement and inheritance should be properly implemented.

## 4. Division by Zero Vulnerability in TokenSender Contract

Summary: 
The `send` function in the `TokenSender` contract contains a division by zero vulnerability when the `scaledPrice` variable is calculated. This is because the `MULTIPLIER_DENOMINATOR` constant is never assigned a value, and therefore always has the default value of zero.

Impact: 
This vulnerability allows an attacker to divide by zero and cause the contract to become unusable.

Recommendation: 
Assign a non-zero value to the `MULTIPLIER_DENOMINATOR` constant in the contract to prevent the division by zero vulnerability. Alternatively, the `scaledPrice` variable can be calculated using a different method that does not involve division by the `MULTIPLIER_DENOMINATOR` constant.

# 16th report for : WithdrawHook.sol https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol

## 1. Global withdraw limit not being enforced

Summary: 
The `globalWithdrawLimitPerPeriod` variable is not being checked before incrementing the `globalAmountWithdrawnThisPeriod` variable in the `hook()` function. This can allow users to withdraw an unlimited amount of funds.

Impact: 
An attacker could withdraw an unlimited amount of funds, potentially causing significant financial losses.

Recommendation: 
To fix this issue, the `globalWithdrawLimitPerPeriod` variable should be checked before incrementing the `globalAmountWithdrawnThisPeriod` variable in the `hook()` function. This will ensure that the global withdraw limit is properly enforced.

## 2. Incorrect Period Length in Withdrawal Limits

Summary: 
The `lastGlobalPeriodReset` and `lastUserPeriodReset` variables in the `WithdrawHook` contract are not updated correctly, which can result in the withdrawal period length being incorrect and potentially allowing users to bypass withdrawal limits.

Impact: 
If the `lastGlobalPeriodReset` and `lastUserPeriodReset` variables are not updated correctly, users may be able to bypass the withdrawal limits set by the `globalWithdrawLimitPerPeriod` and `userWithdrawLimitPerPeriod` variables. This can result in users withdrawing more funds than intended, potentially leading to financial losses for the contract owner and other users.

Recommendation: 
Update the `lastGlobalPeriodReset` and `lastUserPeriodReset` variables to correctly track the withdrawal period length. This can be done by including the period length in the timestamp of the last reset, or by using a separate variable to track the period length. Additionally, consider adding additional checks to ensure that the withdrawal period length is correctly calculated and enforced.

## 3. Withdrawal Hook contract has a critical vulnerability

Summary: 
The `hook()` function in the WithdrawHook contract does not enforce the user-level withdrawal limits. The `if` condition in the `hook()` function enforces the global period reset and limit, but the `else` clause that should enforce the user period reset and limit is never executed because the `if` condition is always `true` due to the lack of `else if` statement. As a result, any user can withdraw an unlimited amount of funds from the contract without any restrictions.

Impact: 
This vulnerability allows any user to withdraw an unlimited amount of funds from the contract without any restrictions. This can result in significant financial loss to the contract owner and other users of the contract.

Recommendation: 
To fix this vulnerability, replace the `else` clause in the `hook()` function with an `else if` statement that checks whether the user period has expired and enforces the user period reset and limit if it has. This will ensure that the user-level withdrawal limits are enforced correctly. Additionally, the function should be tested thoroughly to ensure that it behaves as expected and enforces the withdrawal limits correctly.

## 4. Global Withdraw Limit Not Reset When Period Expires

Summary:
In the `hook` function, when the global period expires, the `lastGlobalPeriodReset` timestamp is updated, but the `globalAmountWithdrawnThisPeriod` variable is not reset to zero. As a result, the global withdraw limit is never reset and will continue to accumulate until it exceeds `globalWithdrawLimitPerPeriod`, causing the require statement on line 59 to fail.

Impact:
Users will not be able to make withdrawals from the contract if the global withdraw limit is exceeded, even if the global period has expired and the limit should have been reset. This can cause user frustration and loss of funds.

Recommendation:
To fix this issue, the `globalAmountWithdrawnThisPeriod` variable should be reset to zero when the global period expires. This can be done by adding the following line of code after the `lastGlobalPeriodReset` variable is updated on line 52:
```
globalAmountWithdrawnThisPeriod = 0;
```