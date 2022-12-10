QA1: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L65
The withdraw function assumes that the collatoral token has 18 decimals, as a result, the withdraw function will not work when the collatoral has different number of decimals. 

QA2: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L45
A common mistake is that user might specify the ``collatoral``'s contract address as the recipient address, so it would be helpful to check to make sure that is not the case
```
require(_recipient != address(this) && _recipient != address(0x0), "wrong recipient address")
``` 

QA3: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L49
The function assumes that an approval from ``msg.sender`` has been made before the function call. This assumption needs to be made explicit as a documentation for the function; otherwise, the function will revert .

QA4: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/TokenSender.sol#L50
A range check is necessary for ``__priceMultiplier``, for example, ``__priceMultiplier <= MULTIPLIER_DENOMINATOR``. 

QA5: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L85
Zero address check is needed so that fee will not be lost to the zero address.

QA6: lock all contracts at the most recent version of solidity, 0.8.17.

QA7: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarket.sol#L65
It is assumed that an allowance has been approved by ``msg.sender`` to ``PrePOMarket`` before the function call. Such assumption needs to be made explicit in documentation - otherwise, the function will revert. 

QA8: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarket.sol#L70-L71
It is assumed that both the collatoral token and the LongShortTokens have 18 decimals. This assumption needs to be explicit. 

QA9: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarket.sol#L65
Needs to check zero amount for ``_amount``. 



