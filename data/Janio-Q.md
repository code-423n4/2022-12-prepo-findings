## Shadow Local Variable 

The local variable `_treasury` in the `RedeemHook.setTreasury(_treasury)`  function is shadowing the state variable `_treasury` in the contract `TokenSenderCaller`.

## Proof of Concept



```solidity
//file: prepo-monorepo-feat-2022-12-prepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol
contract TokenSenderCaller is ITokenSenderCaller {
  address internal _treasury;
  ITokenSender internal _tokenSender;

...
}
```

```solidity
//file: prepo-monorepo-feat-2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol
function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
```

## Recommended Mitigation Steps
Consider renaming the `_treasury` local variable that shadow the `_treasury` state variable.



## Missing checks for `address(0)` for `_governance` local variable when calls `transferOwnership(_governance)`.

The `_setRoleNominee(...) function is called by `revokeNomination(...)` and `grantRole(...)` functions. However, there is no checking for `address(0)` as presented in the code below:


```solidity
//file: apps/smart-contracts/core/contracts/PrePOMarket.sol

  function grantRole(bytes32 _role, address _account) public virtual override onlyRole(getRoleAdmin(_role)) {
    _setRoleNominee(_role, _account, true);
  }

  function revokeNomination(bytes32 _role, address _account) public virtual override onlyRole(getRoleAdmin(_role)) {
    _setRoleNominee(_role, _account, false);
  }

  function _setRoleNominee(
    bytes32 _role,
    address _account,
    bool _nominationStatus
  ) internal virtual {
    _roleToAccountToNominated[_role][_account] = _nominationStatus;
    emit RoleNomineeUpdate(_role, _account, _nominationStatus);
  }
```
Consider checking for `address(0)` before writing in the storage.