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

 G6: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L22
Changing memory variables to calldata vairables can save gas.
```
function createMarket(string calldata _tokenNameSuffix, string calldata _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {
  
```

G7 https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L41
Changing memory variables to calldata vairables can save gas.
```

  function _createPairTokens(string calldata _tokenNameSuffix, string calldata _tokenSymbolSuffix, bytes32 _longTokenSalt, bytes32 _shortTokenSalt) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {
   
```

G8 https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L53
Checking whether _fee is equal to zero will save gas:
```
if(_Fee > 0)  withdrawHook.hook(msg.sender, _baseTokenAmount, _baseTokenAmountAfterFee);
```
In this way, we do not have to check whether ``_fee > 0`` again inside the ``hookd()`` function.

G9: https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/Collateral.sol#L80
might as well check whether ``_amount == 0 `` to save gas.




