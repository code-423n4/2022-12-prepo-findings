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