**packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol**
- L26/60 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L23/26 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L25/37/58 - It is not necessary to create a variable and set a value when we want to set its default value, this is so since when creating the variable it already has that value

- L23 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS


**apps/smart-contracts/core/contracts/Collateral.sol**
- L47/48/67/68 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L47/48/67/68/91/97 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**apps/smart-contracts/core/contracts/DepositHook.sol**
- L48 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L28/44/45 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**apps/smart-contracts/core/contracts/DepositRecord.sol**
- L19/29/30 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**apps/smart-contracts/core/contracts/PrePOMarket.sol**
- L43/44/45/66/67/77/78/85/91/92/120/121/127 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L91 - It is less expensive to validate that uint != 0 than to validate uint > 0


**apps/smart-contracts/core/contracts/WithdrawHook.sol**
- L33/58/63/70 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L75 - It is less expensive to validate that uint != 0 than to validate uint > 0
