**apps/smart-contracts/core/contracts/LongShortToken.sol**
- L6 - ReentrancyGuard is imported, but it is never used, so it should be removed.


**apps/smart-contracts/core/contracts/PrePOMarket.sol**
- L6 - The ERC20 contract is imported, but the contract itself is not used, rather it is used to use the IERC20 interface. This is unnecessary. IERC20 could be directly imported.


**apps/smart-contracts/core/contracts/PrePOMarketFactory.sol**
- L7 - IERC20 is imported, but it is never used, therefore it should be removed.


**packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol**
- L14 - When trying to validate _requiredScore == 0 || getAccountScore(account) >= _requiredScore, the first validation ends up being redundant, since if _requiredScore were 0, the second validation will always return true.
