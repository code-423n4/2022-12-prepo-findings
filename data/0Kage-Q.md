Code Documentation errors

Inline documentation of code has following minor errors that can be corrected

1. Natspec for `IPrePOMarket` event has [duplicate entries](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L20)

2. Parameter definition for `amountAfterFee` in [`Redemption` event](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/interfaces/IPrePOMarket.sol#L39) is incorrect

3. Comments in [`IMarketHook`](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/interfaces/IMarketHook.sol#L16) incorrectly label parameters

_Recommendations_

1. Remove a duplicate line in Natspec ` * @param shortToken Market Short token address` in line 20 of `IPrePOMarket`
2. Replace ` * @param amountAfterFee The amount of Long/Short tokens minted` with ` * @param amountAfterFee The amount of Long/Short tokens redeemed` in line 39 of `IPrePOMarket`
3. Replace `amountBeforeFee` with `amountAfterFee` in line 16 of `IMarketHook`