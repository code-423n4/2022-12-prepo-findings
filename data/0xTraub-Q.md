Ownership can pause protocol to prevent redemptions, making all tokens for a given market worthless by virtue of not being redeemable for collateral. 

The `redeem()` function includes a check that `if (address(_redeemHook) != address(0))` to call `_redeemHook.hook`. However, `setRedeemHook` does not check that the new redeemHook address actually implements that function. As a result the ownership can set a non-zero address that does not implement the function. The external call will fail and any redemptions will as well since the hook needs to be successfully called before redemptions can be processed. 

   Solution: Implement a `supportsInterface(IMarketHook)` check on any new redeemHook addresses being set by ownership