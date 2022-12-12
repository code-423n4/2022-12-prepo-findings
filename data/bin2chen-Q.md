1:
TOKEN TRANSFERS DO NOT VERIFY THAT THE TOKENS WERE SUCCESSFULLY TRANSFERRED
Some tokens (like zrx) do not revert the transaction when the transfer/transferfrom fails and return false, which requires us to check the return value after calling the transfer/transferfrom function.

Use SafeERC20’s safeTransfer/safeTransferFrom functions

```solidity
  function deposit(address _recipient, uint256 _amount) external override nonReentrant returns (uint256) {
    uint256 _fee = (_amount * depositFee) / FEE_DENOMINATOR;
    if (depositFee > 0) { require(_fee > 0, "fee = 0"); }
    else { require(_amount > 0, "amount = 0"); }
    baseToken.transferFrom(msg.sender, address(this), _amount);
+   baseToken.safeTransferFrom(msg.sender, address(this), _amount);
```

2: add check address != 0 ,  Avoid Token Loss
```solidity
  function managerWithdraw(uint256 _amount) external override onlyRole(MANAGER_WITHDRAW_ROLE) nonReentrant {
    if (address(managerWithdrawHook) != address(0)) managerWithdrawHook.hook(msg.sender, _amount, _amount);
+   require(manager!=address(0));
    baseToken.transfer(manager, _amount);
  }

  function hook(address _sender, uint256 _amountBeforeFee, uint256 _amountAfterFee) external override onlyCollateral {
    require(depositsAllowed, "deposits not allowed");
    if (!_accountList.isIncluded(_sender)) require(_satisfiesScoreRequirement(_sender), "depositor not allowed");
    depositRecord.recordDeposit(_sender, _amountAfterFee);
    uint256 _fee = _amountBeforeFee - _amountAfterFee;
-   if (_fee > 0) {
+   if (_fee > 0 && _treasury!=address(0)) {
      collateral.getBaseToken().transferFrom(address(collateral), _treasury, _fee);
      _tokenSender.send(_sender, _fee);
    }
  }  
```