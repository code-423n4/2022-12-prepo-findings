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

## State variable is written after the external calls

In `deposit(...) function the external calls comes before `_mint()`. 

```solidity
  function deposit(address _recipient, uint256 _amount) external override nonReentrant returns (uint256) {
    uint256 _fee = (_amount * depositFee) / FEE_DENOMINATOR;
    if (depositFee > 0) { require(_fee > 0, "fee = 0"); }
    else { require(_amount > 0, "amount = 0"); }
    baseToken.transferFrom(msg.sender, address(this), _amount);
    uint256 _amountAfterFee = _amount - _fee;
    if (address(depositHook) != address(0)) {
      baseToken.approve(address(depositHook), _fee);
      depositHook.hook(_recipient, _amount, _amountAfterFee);
      baseToken.approve(address(depositHook), 0);
    }
    /// Converts amount after fee from base token units to collateral token units.
    uint256 _collateralMintAmount = (_amountAfterFee * 1e18) / baseTokenDenominator;
    _mint(_recipient, _collateralMintAmount);
    emit Deposit(_recipient, _amountAfterFee, _fee);
    return _collateralMintAmount;
  }
```

**Recommendation:** As good practice, consider applying Check-Effects-Interactions Pattern by calling `_mint(...)` before `. This change does not require heavy refactoring.

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

## State Variable updated without checking for address(0) - if the checking is included it can be done using assembly to spare more gas

[`Collateral.sol#L85-L86`](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L85-L86)
```solidity
function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
    manager = _newManager;
    ...
}
```

**The above function `setTreasury` called by contracts who inherit from `TokenSenderCaller` also don't check for address(0)**
[`TokenSenderCaller.sol#L11-L12`](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol#L11-L12)
```solidity
function setTreasury(address treasury) public virtual override {
    _treasury = treasury;
    ...
}
```

[`DepositRecord.sol#L28-L32`](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/DepositRecord.sol#L28-L32)
```solidity
function recordDeposit(address _sender, uint256 _amount) external override onlyAllowedHooks {
    require(_amount + globalNetDepositAmount <= globalNetDepositCap, "Global deposit cap exceeded");
    require(_amount + userToDeposits[_sender] <= userDepositCap, "User deposit cap exceeded");
    globalNetDepositAmount += _amount;
    userToDeposits[_sender] += _amount;
}
```

[`DepositRecord.sol#L50-L51`](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/DepositRecord.sol#L50-L51)
```solidity
function setAllowedHook(address _hook, bool _allowed) external override onlyRole(SET_ALLOWED_HOOK_ROLE) {
    allowedHooks[_hook] = _allowed;
    ...
}
```

[`PrePOMarketFactory.sol#L22-L28`](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22-L28)
```solidity
function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
    require(validCollateral[_collateral], "Invalid collateral");

    (LongShortToken _longToken, LongShortToken _shortToken) = _createPairTokens(_tokenNameSuffix, _tokenSymbolSuffix, longTokenSalt, shortTokenSalt);
    bytes32 _salt = keccak256(abi.encodePacked(_longToken, _shortToken));

    PrePOMarket _newMarket = new PrePOMarket{salt: _salt}(_governance, _collateral, ILongShortToken(address(_longToken)), ILongShortToken(address(_shortToken)), _floorLongPrice, _ceilingLongPrice, _floorValuation, _ceilingValuation, _expiryTime);
    ...
}
```

[`PrePOMarketFactory.sol#L36-L37`](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L36-L37)
```solidity
function setCollateralValidity(address _collateral, bool _validity) external override onlyOwner {
    validCollateral[_collateral] = _validity;
	...
}
```

[`RedeemHook.sol#L30`](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/RedeemHook.sol#L30)
```solidity
function setTreasury(address _treasury) public override onlyOwner { super.setTreasury(_treasury); }
```

[`WithdrawHook.sol#L116`](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/WithdrawHook.sol#L116)
```solidity
function setTreasury(address _treasury) public override onlyRole(SET_TREASURY_ROLE) {
    super.setTreasury(_treasury);
}
```
**Recommendation:** Consider checking for `address(0)` before updating state variables.

### Try using uint instead of bools for storage to avoid overhead

[`AccountList.sol#L9`](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/AccountList.sol#L9)

**Recommendation:** Consider using uint256(1) and uint256(2) for `true` and `false` to avoid wasting gas when changing from `false` to `true`, after having been `true` in the past