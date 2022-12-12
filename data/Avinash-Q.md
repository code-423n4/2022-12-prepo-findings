- https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol

The setAccountList is not restricted to onlyOwner as a result any other can call the setAccountList and change the accountList
Following code snippet can be used
```
  function setAccountList(IAccountList accountList) public onlyOwner virtual override {
    _accountList = accountList;
    emit AccountListChange(accountList);
  }
```

the contract does not check the return value of the getAccountList function before returning it to the caller. This means that if the _accountList variable has not been set, the contract will return a null value, which could cause issues when the calling contract attempts to use the returned value.
Following code snippet can be used
```
  function getAccountList() external view override returns (IAccountList) {
    // Check the value of the _accountList variable before returning it
    require(_accountList != null, "AccountList not set");
    return _accountList;
  }
```

