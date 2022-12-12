## Shadow Local Variables 

The `TokenSenderCaller` contract contains two state variables as described below:

```solidity
//file: prepo-monorepo-feat-2022-12-prepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol
contract TokenSenderCaller is ITokenSenderCaller {
  address internal _treasury;
  ITokenSender internal _tokenSender;

...
}
```

The local variable `_treasury` in the `RedeemHook.setTreasury(_treasury)`  function is shadowing the state variable `_treasury` in the contract `TokenSenderCaller`.

```solidity
//file: apps/smart-contracts/core/contracts/RedeemHook.sol
function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
```

Similarly, the local variables `_treasury` and `_tokenSender` in `DepositHook` contract are shadowing the state variables `_treasury`  and `_tokenSender` in `TokenSenderCaller` contract.


```solidity
//file: apps/smart-contracts/core/contracts/DepositHook.sol
function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) { super.setTreasury(_treasury); }

function setTokenSender(ITokenSender _tokenSender) public override onlyRole(SET_TOKEN_SENDER_ROLE) { super.setTokenSender(_tokenSender); }

```

**Recommendation:** Consider renaming the `_treasury` local variable that shadows the `_treasury` state variable.

## Missing inheritance

`LongShortToken` should inherit from `ILongShortToken`.

```
Contract: apps/smart-contracts/core/contracts/LongShortToken.sol

Interface: apps/smart-contracts/core/contracts/interfaces/ILongShortToken.sol
```

**Recommendation:** Consider inheriting from the missing interface.

---

## Missing checks for `address(0)` for `_account` address

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
**Recommendation:** Consider checking for `address(0)` before writing in the storage.




