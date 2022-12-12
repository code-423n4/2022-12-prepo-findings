#### Redundant nonReentrant modifier
`nonReentrant` modifier could be removed after functions execution order changes to save Gas.
`PrePOMarket.mint` interacts with contracts, owned by the client, except `collateral`. `collateral.transferFrom(msg.sender, address(this), _amount)` should be moved to the last line before `emit Mint`, to prevent potential reentrancy.
`PrePOMarket.redeem`
`Collateral.managerWithdraw` interacts with `managerWithdrawHook`, owned by the client, so external call to `baseToken` would cause no problems.
`Collateral.withdraw` interacts with `baseToken` after state changes, so reentrancy not dangerous.
`Collateral.deposit` interacts with `baseToken` before state changes, but it’s safe to move this block after `_mint` and before `emit Deposit` to prevent any damage from reentrancy:
```
 baseToken.transferFrom(msg.sender, address(this), _amount);
 if (address(depositHook) != address(0)) {
      baseToken.approve(address(depositHook), _fee);
      depositHook.hook(_recipient, _amount, _amountAfterFee);
      baseToken.approve(address(depositHook), 0);
}
```
