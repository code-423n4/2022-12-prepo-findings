--- PrePOMarket.sol ---


1. Does not use OpenZeppelin SafeERC20 for IERC20
    `collateral.transferFrom(msg.sender, address(this), _amount);` in `mint` does not include validation that the transferFrom succeeded.
    If the function does not revert on failure but returns "false" then the user may still mint tokens without supplying collateral
    Solution: Use safeTransferLib or require(success) on transferFrom

2. Should use SafeApprove for collateral approvals in the event where non-standard approval requires first zero-ing out approvals to increase or to use increaseAllowance

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

