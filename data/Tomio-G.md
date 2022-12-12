1.
Title: Gas savings for using solidity 0.8.10

impact:
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

Proof of Concept:
All contract in scope

Recommended Mitigation Steps:
Consider to upgrade pragma to at least 0.8.10.

Solidity 0.8.10 has a useful change which reduced gas costs of external calls
Reference: [here](https://blog.soliditylang.org/2021/11/09/solidity-0.8.10-release-announcement/)
______________________________________________________________________

2.
Title: Reduce the size of error messages (Long revert Strings)

Impact:
Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.
Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

Proof of Concept:
[NFTScoreRequirement.sol#L23](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L23)

Recommended Mitigation Steps:
Consider shortening the revert strings to fit in 32 bytes
________________________________________________________________________

3.
Title: Gas improvement on returning value

Proof of Concept:
[NFTScoreRequirement.sol#L56](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L56)

Recommended Mitigation Steps:
by set `score` in returns L#55 and delete L#56 can save gas

```
function getAccountScore(address account) public view virtual override returns (uint256 score) { //@audit-info: set here
```
________________________________________________________________________

4.
Title: Expression for `constant` values such as a call to `keccak256()`, should use `immutable` rather than `constant`

Proof of Concept:
[Collateral.sol#L21-L27](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L21-L27)
[DepositHook.sol#L17-L25](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L17-L25)

Recommended Mitigation Steps:
Change from `constant` to `immutable`
reference: [here](https://github.com/ethereum/solidity/issues/9232)
________________________________________________________________________