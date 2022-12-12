# Introduction

The PrePO code base is well-written and employs many common gas optimizations in sensible places. It was quite a challenge to find improvements 👍.

# Summary

| ID   | Finding                                               | Instances |
| ---- | ----------------------------------------------------- | --------- |
| G-01 | Single-child inheritance hierarchies can be flattened | 1         |
| G-02 | `x = x + y` is more efficient than `x += y;`          | 5         |
| G-03 | Use named variables in function returns               | 2         |
| G-04 | Redundant `return` statement                          | 2         |
| G-05     | Cache variables in hot loops                                                      | 1          |

# Gas Optimization Findings

## \[G-01\] Single-child inheritance hierarchies can be flattened

Deployment Gas saved:  ✔
Transaction Gas saved:  ✔

### `NFTScoreRequirement` has only one descendant

`DepositHook` is the only class to inherit from `NFTScoreRequirement`.  While it is good practice to separate concerns like this, it could be argued that this is a premature optimization until a second child contract has actually been implemented. Given that `DepositHook` is a hot path for PrePO, and hooks cannot be set by 3rd parties, I would consider flattening this hierarchy for the deployment and transaction (JUMP/stack) gas savings until there is a second descendant of `NFTScoreRequirement`.

* packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol
```solidity
7: contract NFTScoreRequirement is INFTScoreRequirement {
```

* apps/smart-contracts/core/contracts/DepositHook.sol
```solidity
12: contract DepositHook is IDepositHook, AccountListCaller, NFTScoreRequirement, TokenSenderCaller, SafeAccessControlEnumerable {
```

---

## \[G-02\] `x = x + y` is more efficient than `x += y;`

Deployment Gas saved:  ✔
Transaction Gas saved:  ❌

* apps/smart-contracts/core/contracts/DepositRecord.sol
```solidity
31:     globalNetDepositAmount += _amount;
32:     userToDeposits[_sender] += _amount;
```

* apps/smart-contracts/core/contracts/WithdrawHook.sol
```solidity
64:       globalAmountWithdrawnThisPeriod += _amountBeforeFee;
```

* apps/smart-contracts/core/contracts/WithdrawHook.sol
```solidity
71:       userToAmountWithdrawnThisPeriod[_sender] += _amountBeforeFee;
```

* packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol
```solidity
60:       score += IERC721(collection).balanceOf(account) > 0 ? collectionScore : 0;
```

---

## \[G-03\] Use named variables in function returns

Deployment Gas saved:  ✔
Transaction Gas saved:  ❌

Declaring additional variables rather than using named return variables is a waste of deployment gas.

* apps/smart-contracts/core/contracts/Collateral.sol
```solidity
45:   function deposit(address _recipient, uint256 _amount) external override nonReentrant returns (uint256) {
...
60:     uint256 _collateralMintAmount = (_amountAfterFee * 1e18) / baseTokenDenominator;
...
63:     return _collateralMintAmount;
64:   }
```

* packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol
```solidity
55:   function getAccountScore(address account) public view virtual override returns (uint256) {
56:     uint256 score;
...
65:     return score;
66:   }
```

---

## \[G-04\] Redundant `return` statement

Deployment Gas saved:  ✔
Transaction Gas saved:  ❌

If you set values for all named variable in a function return, it is not necessary to use the `return` statement, see line 71 below:

* apps/smart-contracts/core/contracts/PrePOMarketFactory.sol
```solidity
59:   function _createPairTokens(
...
64:   ) internal returns (LongShortToken _newLongToken, LongShortToken _newShortToken) {
...
69:     _newLongToken = new LongShortToken{salt: _longTokenSalt}(_longTokenName, _longTokenSymbol);
70:     _newShortToken = new LongShortToken{salt: _shortTokenSalt}(_shortTokenName, _shortTokenSymbol);
71:     return (_newLongToken, _newShortToken); // <-- remove
72:   }
```

Alternatively, if you are just returning the default value, it is also redundant:

* packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol
```solidity
50:   function getCollectionScore(IERC721 collection) external view virtual override returns (uint256) {
51:     if (_collectionToScore.contains(address(collection))) return _collectionToScore.get(address(collection));
52:     return 0; // <-- remove
53:   }
```

---

## \[G-05\] Cache variables in hot loops

Deployment Gas saved:  ✔
Transaction Gas saved:  ✔

* packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol

Before:

```
25:     for (uint256 i = 0; i < numCollections; ) {
26:       require(scores[i] > 0, "score == 0");
27:       _collectionToScore.set(address(collections[i]), scores[i]);
28:       unchecked {
29:         ++i;
30:       }
31:     }
```

```
·-----------------------------------------------------------|---------------------------|-------------|-----------------------------·
|                    Solc version: 0.8.7                    ·  Optimizer enabled: true  ·  Runs: 200  ·  Block limit: 30000000 gas  │
····························································|···························|·············|······························
|  Methods                                                                                                                          │
························|···································|·············|·············|·············|···············|··············
|  Contract             ·  Method                           ·  Min        ·  Max        ·  Avg        ·  # calls      ·  eur (avg)  │
························|···································|·············|·············|·············|···············|··············
|  DepositHook          ·  setCollectionScores              ·     118079  ·     118307  ·     118155  ·            3  ·          -  │
························|···································|·············|·············|·············|···············|··············
```

After:

```solidity
25:     uint256 score;
26:     for (uint256 i = 0; i < numCollections; ) {
27:       score = scores[i];
28:       require(score > 0, "score == 0");
29:       _collectionToScore.set(address(collections[i]), score);
30:       unchecked {
31:         ++i;
32:       }
33:     }
```

```
·-----------------------------------------|---------------------------|-------------|-----------------------------·
|           Solc version: 0.8.7           ·  Optimizer enabled: true  ·  Runs: 200  ·  Block limit: 30000000 gas  │
··········································|···························|·············|······························
|  Methods                                                                                                        │
··················|·······················|·············|·············|·············|···············|··············
|  Contract       ·  Method               ·  Min        ·  Max        ·  Avg        ·  # calls      ·  eur (avg)  │
··················|·······················|·············|·············|·············|···············|··············
|  DepositHook    ·  setCollectionScores  ·     118043  ·     118271  ·     118119  ·            3  ·          -  │
··················|·······················|·············|·············|·············|···············|··············
```
