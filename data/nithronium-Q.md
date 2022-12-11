https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/AccountList.sol#L13

```
function set(address[] calldata _accounts, bool[] calldata _included) external override onlyOwner {
    require(_accounts.length == _included.length, "Array length mismatch");
    uint256 _arrayLength = _accounts.length;
    for (uint256 i; i < _arrayLength; ) {
      _resetIndexToAccountToIncluded[resetIndex][_accounts[i]] = _included[i];
      unchecked {
        ++i;
      }
    }
  }
```

The above piece of code enables owner to set a certain addresses as included/not included to the list. However later when the owner does a reset - it defaults to true on all entries.

https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/AccountList.sol#L24

By checking the context of the code, after one reset there is no way to set an address' bool value as `false` , since this is intended and list works only as whitelist, it is better to optimize the initial `set` code to this;

```
  function set(address[] calldata _accounts) external override onlyOwner {
    uint256 _arrayLength = _accounts.length;
    for (uint256 i; i < _arrayLength; ) {
      _resetIndexToAccountToIncluded[resetIndex][_accounts[i]] = true;
      unchecked {
        ++i;
      }
    }
  }
```
