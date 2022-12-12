# 1. inadequate NatSpec

Solidity contracts can use the Ethereum Natural Language Specification Format (NatSpec) to provide detailed documentation for functions, return variables, and other elements of the contract. This is done using a special type of comment within the contract code.

An instance of inadequate NatSpec: [AccountListCaller.sol](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol)

Here is an example of a NatSpec comment in a Solidity contract:

```solidity
/// @notice This is a NatSpec comment that provides
/// a brief description of the `setAccountList` function.
function setAccountList(IAccountList accountList) public virtual override {
  ...
}
```

NatSpec comments start with `///` and follow a specific format that includes tags such as `@notice` and `@param` to indicate the type of information being provided. You can find more information about NatSpec and its usage in Solidity contracts at the following link:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

# 2. Typos

- ##   Typo 1

[File: IAllowedMsgSenders.sol](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/IAllowedMsgSenders.sol)

[Line 24](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/IAllowedMsgSenders.sol#L24)

```
Line 24:    * @dev This function is meant to be overriden and does not include any
```

---

- ##   Typo 2

[File: INFTScoreRequirement.sol](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol)

[LIne 28](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol#L28), [LIne 40](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol#L40), [LIne 49](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol#L49)

```
Line 28:    * @dev This function is meant to be overriden and does not include any

Line 40:    * This function is meant to be overriden and does not include any

Line 49:    * @dev This function is meant to be overriden and does not include any
```

---

- ##  Typo 3 

[File: ITokenSenderCaller.sol](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol)

[Line 27](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol#L27), [Line 35](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/interfaces/ITokenSenderCaller.sol#L35)

```
Line 27:    * @dev This function is meant to be overriden and does not include any

Line 35:    * @dev This function is meant to be overriden and does not include any
```

---

- ##   Typo 4

[File: ICollateral.sol](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol)

[Line 166](https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol#L166)

```
Line 166:    /// @return The factor used to calculate fees for depositinng
```

---

Suggested changes:

- `overriden` => `overridden`
- `depositinng` => `depositing`