## No. 1
Setting the critical variables (roles) should be done in two steps. Regarding the following function:
```
function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
    manager = _newManager;
    emit ManagerChange(_newManager);
  }
```
It should be modified as:
```
function setPendingManager(address _newPendingManager)
        external
        override
        onlyRole(SET_MANAGER_ROLE)
    {
        pendingManager = _newPendingManager;
    }

    function acceptMangerTransfership() external {
        require(msg.sender == pendingManager);
        manager = pendingManager;
        pendingManager = address(0);
    }
```