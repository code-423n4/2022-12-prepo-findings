# QA Report

## Summary

|               | Issue         | Risk     | Instances     |
| :-------------: |:-------------|:-------------:|:-------------:|
| 1      | Front-runable `initialize` function | Low | 1 |
| 2      | Manager can drain funds from Collateral contract | Low | 1 |
| 3      | Setters should check the input value and revert if it's the zero address or zero | Low | / |
| 4      | Use scientific notation | NC | 7 |


## Findings

### 1- Front-runable `initialize` function :

The `initialize` function inside the Collateral contract is used to initialize the contract  access control and important contract parameters, but any attacker can initialize the contract before the legitimate deployer and even if the developers when deploying call immediately the `initialize` function, malicious agents can trace the protocol deployment transactions and insert their own transaction between them and by doing so they front run the developers call and gain the ownership of the contract and set the wrong parameters.

The impact of this issue is : 

* In the best case developers notice the problem and have to redeploy the contract and thus costing more gas.

* In the worst case the protocol continue to work with the wrong owner and state parameters which could lead to the loss of user funds.

#### Risk : Low 

#### Proof of Concept

Instances include:

File: apps/smart-contracts/core/contracts/Collateral.sol

[function initialize(string memory _name, string memory _symbol) public initializer](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L34)

#### Mitigation

It's recommended to use the constructor to initialize non-proxied contracts.

For initializing proxy (upgradable) contracts deploy contracts using a factory contract that immediately calls initialize after deployment or make sure to call it immediately after deployment and verify that the transaction succeeded.


### 2- Manager can drain funds from Collateral contract :

The `managerWithdraw` function inside the Collateral contract is used to allow the manager to transfer to him self a certain amount of `baseToken` but it can be used to drain all the contract baseToken balance if for example the manager and withdrawal_manager were malicious, and in the case the users who deposited tokens will not be able to get them back.

#### Risk : Low 

#### Proof of Concept

The issue occurs in the function below :

File: apps/smart-contracts/core/contracts/Collateral.sol [Line 80-83](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L80-L83)
```
function managerWithdraw(uint256 _amount) external override onlyRole(MANAGER_WITHDRAW_ROLE) nonReentrant {
    if (address(managerWithdrawHook) != address(0)) managerWithdrawHook.hook(msg.sender, _amount, _amount);
    baseToken.transfer(manager, _amount);
}
```

If the withdrawal manager and the manager are both malicious agent they can use this function to transfer any amount of baseToken to the manager account.

#### Mitigation

It's recommended to use a DAO governance to control funds withdrawal from the Collateral contract in case they were needed.


### 3- Setters should check the input value and revert if it's the zero address or zero  :

#### Risk : Low

When setting a new value to a state variable the setter function must check the new value and revert if it's the zero address or zero. This issue is present in almost all the setters functions a cross the different contracts

#### Mitigation
Add non-zero address/uint checks for the setters functions.

### 4- Use scientific notation :

When using multiples of 10 you shouldn't use decimal literals or exponentiation (e.g. 1000000, 10**18) but use scientific notation for better readability.

#### Risk : NON CRITICAL

#### Proof of Concept
Instances include:

File: apps/smart-contracts/core/contracts/Collateral.sol [Line 19-20](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L19-L20)
```
uint256 public constant FEE_DENOMINATOR = 1000000;
uint256 public constant FEE_LIMIT = 100000;
```

File: apps/smart-contracts/core/contracts/DepositTradeHelper.sol [Line 12](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L12)
```
uint24 public constant override POOL_FEE_TIER = 10000;
```

File: apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol [Line 12](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol#L12)
```
uint256 public constant PERCENT_DENOMINATOR = 1000000;
```

File: apps/smart-contracts/core/contracts/PrePOMarket.sol [Line 30-31](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L30-L31)
```
uint256 private constant FEE_DENOMINATOR = 1000000;
uint256 private constant FEE_LIMIT = 100000;
```

File: apps/smart-contracts/core/contracts/TokenSender.sol [Line 25](https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol#L25)
```
uint256 public constant MULTIPLIER_DENOMINATOR = 10000;
```
