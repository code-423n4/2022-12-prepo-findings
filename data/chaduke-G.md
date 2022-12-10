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
