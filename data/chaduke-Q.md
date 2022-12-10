QA1: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L65
The withdraw function assumes that the collatoral token has 18 decimals, as a result, the withdraw function will not work when the collatoral has different number of decimals. 

QA2: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L45
A common mistake is that user might specify the ``collatoral``'s contract address as the recipient address, so it would be helpful to check to make sure that is not the case
```
require(_recipient != address(this) && _recipient != address(0x0), "wrong recipient address")
``` 

QA3: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L49
The function assumes that an approval from ``msg.sender`` has been made before the function call. This assumption needs to be made explicit as a documentation for the function; otherwise, the function will revert .

