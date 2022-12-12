#### Highly permissive role access
The owner of `PrePOMarket` contract can change at any point in time mint and redeem hooks, which could lead to unexpected behavior. The same is applicable for `Collateral` contract.
`WithdrawHook` can update `collateral`, `depositRecord` contracts implementations.
`DepositHook` can update `collateral` and `depositRecord` contract implementations.
It’s recommended to set the implementations once and not allow further modifications, to keep contract predictability and stability. 

**Path:** ./contract/PrePOMarket.sol;
./contract/Collateral.sol
