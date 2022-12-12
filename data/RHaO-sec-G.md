# [G-1] Bytes constants are more efficient than string constants.

If data can fit into 32 bytes, then you should use bytes32 datatype rather than bytes or strings as it is cheaper in solidity.

[Collateral.sol#L34](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L34)

[PrePOMarketFactory.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22)
[PrePOMarketFactory.sol#L41-L45](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41-L45)

[LongShortToken.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol#L9)

# [G-2] Use calldata instead of memory for function parameters  [ref](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#use-calldata-instead-of-memory-for-function-parameters)

It is generally cheaper to load variables directly from calldata, rather than copying them to memory. Only use memory if the variable needs to be modified.

[PrePOMarketFactory.sol#L22](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22)
[PrePOMarketFactory.sol#L41-L45](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41-L45)

[DepositHook.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L73-L75)

[Collateral.sol#L34](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L34)

[NFTScoreRequirement.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L35)
[NFTScoreRequirement.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L22)

# [G-3] Use function instead of modifiers

This will save gas

[DepositHook.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L27-L29)

```
  modifier onlyCollateral() {
    require(msg.sender == address(collateral), "msg.sender != collateral");
    _;
  }

mitigate this as


  function onlyCollateral() private {
    require(msg.sender == address(collateral), "msg.sender != collateral");
   
  }

and in second function  do this

 function hook(address _sender, uint256 _amountBeforeFee, uint256 _amountAfterFee) external override {
    onlyCollateral();
    require(depositsAllowed, "deposits not allowed");
    if (!_accountList.isIncluded(_sender)) require(_satisfiesScoreRequirement(_sender), "depositor not allowed");
    depositRecord.recordDeposit(_sender, _amountAfterFee);
    uint256 _fee = _amountBeforeFee - _amountAfterFee;
    if (_fee > 0) {
      collateral.getBaseToken().transferFrom(address(collateral), _treasury, _fee);
      _tokenSender.send(_sender, _fee);
    }
  }
```


[WithdrawHook.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L32)

```
  modifier onlyCollateral() {
    require(msg.sender == address(collateral), "msg.sender != collateral");
    _;
  }
```
[L53](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L53)
```
 function hook(
    address _sender,
    uint256 _amountBeforeFee,
    uint256 _amountAfterFee
  ) external override onlyCollateral {

```

# [G-4] x = x + y is more efficient, than x += y 

[DepositRecord.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L31-L32)

to mitigate this 
```
 globalNetDepositAmount  = globalNetDepositAmount + _amount;
 userToDeposits[_sender]  = userToDeposits[_sender] + _amount;
```
[DepositRecord.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol#L36)

mitigate as
```
    if (globalNetDepositAmount > _amount) { globalNetDepositAmount = globalNetDepositAmount - _amount; }
```

[WithdrawHook.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L64)

mitigate as
```
     globalAmountWithdrawnThisPeriod =  globalAmountWithdrawnThisPeriod + _amountBeforeFee;
```

[WithdrawHook.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L71)
```
     userToAmountWithdrawnThisPeriod[_sender] =  userToAmountWithdrawnThisPeriod[_sender] += _amountBeforeFee;

```

