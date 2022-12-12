## [QA-01] Use latest Solidity version with a stable pragma statement
Solidity pragma versioning should be upgraded to latest available version. Currently the solidity version in contracts is 0.8.7 which was found to possess some bugs.

## [QA-02] NOT USING THE LATEST VERSION OF OPENZEPPELIN FROM DEPENDENCIES
The package.json configuration file says that the project is using 1.19.0 of OpenZeppelin which has a not last update version.
```
69    "@openzeppelin/hardhat-upgrades": "1.19.0"
```