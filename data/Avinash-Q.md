- https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol

The setAccountList is not restricted to onlyOwner as a result any other can call the setAccountList and change the accountList
```
  function setAccountList(IAccountList accountList) public virtual override {
    _accountList = accountList;
    emit AccountListChange(accountList);
  }
```

