## 1. CAN USE **LESS THAN OPERATOR(<)** ONLY INSTEAD OF **LESS THAN OR EQUAL OPERATOR (<=)**

No need to check for the equal condition in the both require functions below:

    File: apps/smart-contracts/core/contracts/PrePOMarket.sol
    require(_finalLongPayout >= floorLongPayout, "Payout cannot be below floor");
    require(_finalLongPayout <= ceilingLongPayout, "Payout cannot exceed ceiling");

floorLongPayout is anyway smaller than ceilingLongPayout since it is checked in the constructor.
So it is enough to check for the `_finalLongPayout < ceilingLongPayout` condition in the second require, since equal condition is already checked in the first require.

    require(_finalLongPayout >= floorLongPayout, "Payout cannot be below floor");

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L120-L121


## 2. REDUNDANT IMPORT OF REENTRANCYGUARD.SOL

Even though ReentrancyGuard.sol is imported, it is not used in the scope of the contract. 

    File: apps/smart-contracts/core/contracts/LongShortToken.sol
    import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol#L6

## 3. USE A MORE RECENT VERSION OF SOLIDITY

Can use a solidity version of at least 0.8.12 to get `string.concat()` instead of `abi.encodePacked()`

    string memory _longTokenName = string(abi.encodePacked("LONG", " ", _tokenNameSuffix));
    string memory _shortTokenName = string(abi.encodePacked("SHORT", " ", _tokenNameSuffix));
    string memory _longTokenSymbol = string(abi.encodePacked("L", "_", _tokenSymbolSuffix));
    string memory _shortTokenSymbol = string(abi.encodePacked("S", "_", _tokenSymbolSuffix));

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol#L42-L45