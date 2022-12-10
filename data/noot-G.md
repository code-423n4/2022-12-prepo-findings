`IMarketHook.hook` should accept one `fee` parameter instead of `amountBeforeFee` and `amountAfterFee`.

The only time the `amountBeforeFee` and `amountAfterFee` values are used are in `RedeemHook.hook`, to calculate `fee`:
```
uint256 fee = amountBeforeFee - amountAfterFee;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L19

Instead, `fee` should be passed directly, removing the subtraction in this function, as well as a subtraction in the caller in `PrePOMarket.redeem()`.

```
  function hook(
    address sender,
    uint256 amountBeforeFee,
    uint256 amountAfterFee
  ) external;
```

should be:
```
  function hook(
    address sender,
    uint256 fee
  ) external;
```