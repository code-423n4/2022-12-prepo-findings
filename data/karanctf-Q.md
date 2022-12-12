Use scientific notation (e.g. `10e18`) rather than exponentiation (e.g. `10**18`)
```solidity
apps/smart-contracts/core/contracts/TokenSender.sol:33:    _outputTokenDecimalsFactor = 10**outputToken.decimals();
apps/smart-contracts/core/contracts/Collateral.sol:31:    baseTokenDenominator = 10**_newBaseTokenDecimals;
```
**Mitigation**: Consider refactoring functions to standardise on one return pattern, preferably explicit returns.
check if it is possible to use 10e10 insted

## Open TODOs
**Summary**: In code comments should be removed as they imply that the code is incomplete or buggy, or that the implementation may not have been entirely followed.
```solidity
apps/smart-contracts/core/contracts/TokenSender.sol:15:  WithdrawERC20, // TODO: Access control when WithdrawERC20 updated
```
If open TODOs are not tracked or addressed, it can be difficult to determine the progress and status of the code base, leading to inefficiencies in the development process.