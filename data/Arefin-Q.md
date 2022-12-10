In [AccountListCaller.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol) 
You can use the  "__is__" keyword to check if the "___accountList__"  variable implements the __IAccountList__ interface, which would make the code more readable and maintainable.
```
    function getAccountList() external view override returns (IAccountList) {
    require(_accountList is IAccountList, "Account list is not set or does not implement the IAccountList interface");
    return _accountList;
}
