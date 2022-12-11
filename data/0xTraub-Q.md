--- PrePOMarket.sol ---


1. Does not use OpenZeppelin SafeERC20 for IERC20
    `collateral.transferFrom(msg.sender, address(this), _amount);` in `mint` does not include validation that the transferFrom succeeded.
    If the function does not revert on failure but returns "false" then the user may still mint tokens without supplying collateral
    Solution: Use safeTransferLib or require(success) on transferFrom

2. Should use SafeApprove for collateral approvals in the event where non-standard approval requires first zero-ing out approvals to increase or to use increaseAllowance

3. Decimal Mismatch from OpenZeppelin ERC20. The OpenZeppelin ERC20 token file enforces 18 decimals of precision unless explicitly overwritten. This means all long-short tokens will be default 18-decimals precision. In the mint/redeem functions the amount of tokens minted/burned is based on some amount of collateral, I.E if USDC/USDT, which has 6-decimals of precision, is used as collateral to mint, then 1e6 amount of long-short tokens will be minted to the user. This means that there is now a mismatch between number of decimals stated in the ERC20 `decimals()` method, and the actual number of decimals used for tracking balances of tokens. Given that external protocols may do calculations based on the stated number of decimals to save precision, this can lead to misc. rounding-errors. It may also complicated/break front ends which rely on proper decimal numbers to display properly formatted balances to users.

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L70-L71
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol#L87-L89

Solution: Modify LongShortToken.sol to set the number of decimals to the same as the collateral number, or do dynamic scaling at the time of mint/redeem to scale from collateral-decimals up/down to 18 when non-18-decimal collateral is used.

--- Collateral.sol ---

1. Ownership can pause protocol to prevent redemptions, making all tokens for a given market worthless by virtue of not being redeemable for collateral. 

The `redeem()` function includes a check that `if (address(_redeemHook) != address(0))` to call `_redeemHook.hook`. However, `setRedeemHook` does not check that the new redeemHook address actually implements that function. As a result the ownership can set a non-zero address that does not implement the function. The external call will fail and any redemptions will as well since the hook needs to be successfully called before redemptions can be processed. 

   Solution: Implement a `supportsInterface(IMarketHook)` check on any new redeemHook addresses being set by ownership

2. `setManager(address _newManager)` does not have a `require(_newManager != address(0))` check
3. Consider using a two-step transfer process that requires the new manager to accept the responsibility to avoid copy-paste errors

--- DepositTradeHelper.sol ---

1. `IERC20Permit(address(_baseToken)).permit(msg.sender, address(this), type(uint256).max, baseTokenPermit.deadline, baseTokenPermit.v, baseTokenPermit.r, baseTokenPermit.s);`

For amount to permit consider using `baseAmount` instead of type(uint256).max, which causes an approval for unlimited tokens. First, depending on the token, this may cause problems if the amount to approve must first be zero since you are not first zero-ing out the allowance. Second, since the user is always signing a permit message it is better for security to only permit the number of tokens needed to complete the transaction, resulting in a more secure allowance of zero outside of the transaction, otherwise they will always be approving the contract to spend max-tokens which costs gas and is unnecesarry. 

2. `if (baseTokenPermit.deadline != 0)`, consider using if `deadline > block.timestamp` to ensure that the deadline is in the future, and preventing any signatures from being rejected by the ERC20 permit function. It also serves the same function since a permit deadline of zero would result in bypassing that statement anyways.

--- PrePOMarketFactory.sol ---

initialize() function can be called by anybody when the contract is not initialized.

More importantly, if someone else runs this function, they will have full authority because of the __Ownable_init() function. Also, there is no 0 address check in the address arguments of the initialize() function, which must be defined.

Solution: Specify that only a pre-defined initializer address is allowed to call the function
`require (msg.sender == INITIALIZER_ADDR);


 --- Misc ---

All events triggered on ownership changing a state-variable should include the original value and the new value. Examples include
1. emit RedemptionFeeChange(_redemptionFee); only emits with new fee, not what it was changed from
2. emit RedeemHookChange(address(redeemHook)) 
3. emit MintHookChange(address(mintHook)) should include the previous address also not just the new one