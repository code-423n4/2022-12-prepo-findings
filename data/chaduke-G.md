G1. https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L13
Dropping the first condition can save gas since the second condition already implies the first. Although this is a read function, it might be called by a write function.
```
 return getAccountScore(account) >= _requiredScore;

```   

G2. https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L22
Changing the arguments to calldata will save gas.
```
function setCollectionScores(IERC721[] calldata collections, uint256[] calldata scores) public virtual override
```

G3. https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L35
Changing the arguments to calldata will save gas.
```
function removeCollections(IERC721[] calldata collections) public virtual override 
```

G4. https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/WithdrawHook.sol#L64
Change it to the following line to save gas:
```
globalAmountWithdrawnThisPeriod = globalAmountWithdrawnThisPeriod+_amountBeforeFee
```

G5. https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarket.sol#L95
After line 94, ``_collateralAllowanceBefore`` is actually equal to ``_expectedFee``, so instead of introducing a new variable ``_collateralAllowanceBefore`` and calculating its values via ``collateral.allowance()``, we can save gas by simply using ``_expectedFee``.

 

