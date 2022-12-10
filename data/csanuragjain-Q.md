## Use safeTransferFrom instead of transferFrom

Contract
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L49

Issue:
While depositing contract is making use of transferFrom function which is insecure and the function fails to check the outcome of the transferFrom function (whether it was success or not)

```
function deposit(address _recipient, uint256 _amount) external override nonReentrant returns (uint256) {
    uint256 _fee = (_amount * depositFee) / FEE_DENOMINATOR;
    if (depositFee > 0) { require(_fee > 0, "fee = 0"); }
    else { require(_amount > 0, "amount = 0"); }
    baseToken.transferFrom(msg.sender, address(this), _amount);
...
}
```

Recommendation:
Use safeTransferFrom which checks the outcome of the transfer and fails if transfer was not success