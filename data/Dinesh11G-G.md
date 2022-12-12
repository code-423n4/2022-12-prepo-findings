### 1st Bug
Don't Initialize Variables with Default Value

#### Impact
Issue Information: 
Uninitialized variables are assigned with the types default value.

Explicitly initializing a variable with it's default value costs unnecessary gas.

Example
🤦 Bad:

uint256 x = 0;
bool y = false;
🚀 Good:

uint256 x;
bool y;

#### Findings:
```
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::187 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::192 => for (uint256 i = 0; i < _stakingContracts.length; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::213 => for (uint256 i = 0; i < stakingContracts.length; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::284 => for (uint256 i = 0; i < dialLen; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::309 => for (uint256 i = 0; i < 16; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::350 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::407 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::441 => for (uint256 i = 0; i < dialLen; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::476 => for (uint256 i = 0; i < dialLen; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::506 => for (uint256 k = 0; k < dialLen; k++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::530 => for (uint256 l = 0; l < dialLen; l++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::553 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::624 => for (uint256 i = 0; i < _preferences.length; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::739 => for (uint256 i = 0; i < 16; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::618 => uint256 i = 0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::182 => uint256 low = 0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::178 => uint256 low = 0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::246 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::284 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::306 => for (uint256 i = 0; i < len2; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::339 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::360 => for (uint256 j = 0; j < len2; j++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::123 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::690 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::98 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::297 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::380 => for (uint256 i = 0; i < _indices.length; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::589 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::742 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::824 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::847 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::877 => for (uint256 i = 0; i < 256; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::879 => for (uint256 j = 0; j < len; j++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::931 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::940 => uint256 b = 0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::126 => for (uint256 i = 0; i < count; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::134 => uint256 cache = 0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::201 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::311 => bool undergoingRecol = false;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::312 => for (uint256 j = 0; j < _bAssetPersonal.length; j++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::155 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::722 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1Migrator.sol::24 => for (uint8 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1Migrator.sol::59 => for (uint8 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::146 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::713 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::99 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::167 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::101 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::312 => uint256 unclaimedSeconds = 0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::489 => uint256 unclaimedSeconds = 0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::83 => uint256 votingPower = 0;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::84 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::104 => for (uint256 i = 0; i < len; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::130 => for (uint8 i = 0; i < 32; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::57 => for (uint256 i = 0; i < bAssetCount; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::65 => for (uint256 i = 0; i < _whitelisted.length; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::261 => for (uint256 i = 0; i < bAssetCount; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenPass.sol::26 => for (uint256 i = 0; i < _arrayLength; ++i) {
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenPass.sol::38 => for (uint256 i = 0; i < _arrayLength; ++i) {
```
#### Tools used
Manual code review

### 2nd Bug
Cache Array Length Outside of Loop

#### Impact
Issue Information: 
Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

Example
🤦 Bad:

for (uint256 i = 0; i < array.length; i++) {
    // invariant: array's length is not changed
}
🚀 Good:

uint256 len = array.length
for (uint256 i = 0; i < len; i++) {
    // invariant: array's length is not changed
}

#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::14 => require(_accounts.length == _included.length, "Array length mismatch");
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::15 => uint256 _arrayLength = _accounts.length;
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::26 => uint256 _arrayLength = _newIncludedAccounts.length;
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::41 => * While we could include the period length in the last reset timestamp,
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol::51 => * @notice Sets the length in seconds for which global withdraw limits will
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/IWithdrawHook.sol::65 => * @notice Sets the length in seconds for which user withdraw limits will
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/AccountList.sol::19 => require(_accounts.length == _included.length, "Array length mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/AccountList.sol::20 => uint256 _arrayLength = _accounts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/AccountList.sol::35 => uint256 _arrayLength = _newIncludedAccounts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/PPO.sol::39 => //solhint-disable-next-line max-line-length
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/PPO.sol::41 => //solhint-disable-next-line max-line-length
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::175 => uint256 len = _recipients.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::177 => _notifies.length == len && _caps.length == len,
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::192 => for (uint256 i = 0; i < _stakingContracts.length; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::213 => for (uint256 i = 0; i < stakingContracts.length; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::281 => uint256 dialLen = dials.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::287 => uint256 voteHistoryLen = dialData.voteHistory.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::348 => uint256 len = dials.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::381 => require(_dialId < dials.length, "Invalid dial id");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::406 => uint256 len = stakingContracts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::434 => uint256 dialLen = _dialIds.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::435 => require(dialLen > 0 && _amounts.length == dialLen, "Invalid inputs");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::443 => require(dialId < dials.length, "Invalid dial id");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::474 => uint256 dialLen = dials.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::483 => uint256 end = dialData.voteHistory.length - 1;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::552 => uint256 len = _dialIds.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::554 => require(_dialIds[i] < dials.length, "Invalid dial id");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::612 => require(_preferences.length <= 16, "Max of 16 preferences");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::624 => for (uint256 i = 0; i < _preferences.length; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::625 => require(_preferences[i].dialId < dials.length, "Invalid dial id");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::637 => if (_preferences.length < 16) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::639 => newDialWeights |= uint256(255) << ((_preferences.length * 16) + 8);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::751 => uint256 len = voteHistory.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::15 => * Scaled balance is determined by quests a user completes, and the length of time they keep the raw balance wrapped.
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::96 => return SafeCast.toUint32(_checkpoints[account].length);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::116 => uint256 pos = _checkpoints[account].length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::157 => uint256 len = _totalSupplyCheckpoints.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::181 => uint256 high = ckpts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::280 => uint256 pos = ckpts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::16 => * Scaled balance is determined by quests a user completes, and the length of time they keep the raw balance wrapped.
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::95 => return SafeCast.toUint32(_checkpoints[account].length);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::112 => uint256 pos = _checkpoints[account].length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::153 => uint256 len = _totalSupplyCheckpoints.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::177 => uint256 high = ckpts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::269 => uint256 pos = ckpts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::17 => * Scaled balance is determined by quests a user completes, and the length of time they keep the raw balance wrapped.
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::200 => _quests.length - 1,
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::214 => require(_id < _quests.length, "Quest does not exist");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::245 => uint256 len = _quests.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::278 => uint256 len = _ids.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::305 => uint256 len2 = _stakedTokens.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::329 => uint256 len = _accounts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::359 => uint256 len2 = _stakedTokens.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::379 => _id < _quests.length &&
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::17 => * Scaled balance is determined by quests a user completes, and the length of time they keep the raw balance wrapped.
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/deps/SignatureVerifier.sol::97 => require(signature.length == 65, "invalid signature length");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::121 => uint256 len = _bAssets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::239 => uint256 len = _inputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::240 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::296 => uint256 len = _inputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::297 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::512 => uint256 len = _outputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::513 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::575 => uint256 len = _outputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::576 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::685 => uint256 len = _bAssets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::859 => require(_min <= 1e18 / (data.bAssetData.length * 2), "Min weight oob");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::860 => require(_max >= 1e18 / (data.bAssetData.length - 1), "Max weight oob");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::93 => uint256 len = _indices.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::294 => uint256 len = cachedBassetData.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::380 => for (uint256 i = 0; i < _indices.length; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::586 => uint256 len = _indices.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::740 => uint256 len = _indices.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::821 => uint256 len = _bAssets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::844 => uint256 len = _x.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::869 => uint256 len = _x.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::926 => uint256 len = _x.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::68 => uint8 bAssetCount = uint8(_bAssetPersonal.length);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::122 => uint256 count = bAssetData_.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::198 => uint256 len = _bAssets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::312 => for (uint256 j = 0; j < _bAssetPersonal.length; j++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::153 => uint256 len = _bAssets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::271 => uint256 len = _inputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::272 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::328 => uint256 len = _inputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::329 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::544 => uint256 len = _outputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::545 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::607 => uint256 len = _outputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::608 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::717 => uint256 len = _bAssets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::891 => require(_min <= 1e18 / (data.bAssetData.length * 2), "Min weight oob");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::892 => require(_max >= 1e18 / (data.bAssetData.length - 1), "Max weight oob");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1Migrator.sol::21 => uint256 len = importedBasket.bassets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1Migrator.sol::56 => revert("Invalid length");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::144 => uint256 len = _bAssets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::262 => uint256 len = _inputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::263 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::319 => uint256 len = _inputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::320 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::535 => uint256 len = _outputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::536 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::598 => uint256 len = _outputQuantities.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::599 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::708 => uint256 len = _bAssets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::882 => require(_min <= 1e18 / (data.bAssetData.length * 2), "Min weight oob");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::883 => require(_max >= 1e18 / (data.bAssetData.length - 1), "Max weight oob");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::94 => uint256 len = _keys.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::96 => require(len == _addresses.length, "Insufficient address data");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::97 => require(len == _isLocked.length, "Insufficient locked statuses");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::164 => uint256 len = _keys.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::36 => /// @notice length of each staking period in seconds. 7 days = 604,800; 3 months = 7,862,400
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::96 => uint256 len = _mAssets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::98 => _savingsContracts.length == len && _revenueRecipients.length == len,
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::478 => * @dev Calculates the seconds of unclaimed rewards, based on period length
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/upgradability/DelayedProxyAdmin.sol::111 => if (data.length == 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::82 => uint256 len = stakingContractsArr.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::55 => uint256 bAssetCount = _bAssets.length;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::56 => require(bAssetCount == _pTokens.length, "Invalid input arrays");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::63 => require(_whitelisted.length > 0, "Empty whitelist array");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::65 => for (uint256 i = 0; i < _whitelisted.length; i++) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::258 => uint256 bAssetCount = bAssetsMapped.length;
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenPass.sol::25 => uint256 _arrayLength = _accounts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenPass.sol::37 => uint256 _arrayLength = _tokenIds.length;
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/PurchaseHook.sol::61 => require(_contracts.length == _amounts.length, "Array length mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/PurchaseHook.sol::62 => uint256 _arrayLength = _contracts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/PurchaseHook.sol::77 => _contracts.length == _amounts.length && _ids.length == _amounts.length,
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/PurchaseHook.sol::78 => "Array length mismatch"
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/PurchaseHook.sol::80 => uint256 _arrayLength = _contracts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::42 => _tokenContracts.length == _prices.length &&
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::43 => _ids.length == _prices.length,
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::44 => "Array length mismatch"
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::46 => uint256 _arrayLength = _tokenContracts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::70 => _tokenContracts.length == _ids.length &&
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::71 => _ids.length == _amounts.length &&
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::72 => _amounts.length == _purchasePrices.length,
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::73 => "Array length mismatch"
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::77 => uint256 _arrayLength = _tokenContracts.length;
prepo-monorepo/apps/smart-contracts/token/contracts/vesting/Vesting.sol::55 => require(_recipients.length == _amounts.length, "Array length mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/vesting/Vesting.sol::57 => uint256 _arrayLength = _recipients.length;
prepo-monorepo/packages/prepo-shared-contracts/contracts/AccountList.sol::14 => require(_accounts.length == _included.length, "Array length mismatch");
prepo-monorepo/packages/prepo-shared-contracts/contracts/AccountList.sol::15 => uint256 _arrayLength = _accounts.length;
prepo-monorepo/packages/prepo-shared-contracts/contracts/AccountList.sol::26 => uint256 _arrayLength = _newIncludedAccounts.length;
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::23 => require(collections.length == scores.length, "collections.length != scores.length");
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::24 => uint256 numCollections = collections.length;
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::36 => uint256 numCollections = collections.length;
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::43 => emit CollectionScoresChange(collections, new uint256[](collections.length));
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::57 => uint256 numCollections = _collectionToScore.length();
prepo-monorepo/packages/prepo-shared-contracts/contracts/WithdrawERC1155.sol::18 => require(_erc1155Tokens.length == _recipients.length && _recipients.length == _ids.length && _ids.length == _amounts.length, "Array length mismatch");
prepo-monorepo/packages/prepo-shared-contracts/contracts/WithdrawERC1155.sol::19 => uint256 _arrayLength = _erc1155Tokens.length;
prepo-monorepo/packages/prepo-shared-contracts/contracts/WithdrawERC721.sol::17 => require(_erc721Tokens.length == _ids.length, "Array length mismatch");
prepo-monorepo/packages/prepo-shared-contracts/contracts/WithdrawERC721.sol::18 => uint256 _arrayLength = _erc721Tokens.length;
```
#### Tools used
Manual code review

### 3rd Bug
Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
Issue Information: 
When dealing with unsigned integer types, comparisons with != 0 are cheaper than with > 0.

Example
🤦 Bad:

// `a` being of type unsigned integer
require(a > 0, "!a > 0");
🚀 Good:

// `a` being of type unsigned integer
require(a != 0, "!a > 0");

#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol::85 => * a fee of 0 (if the deposit fee factor is > 0), including 0.
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol::101 => * a fee of 0 (if the withdraw fee factor is > 0), including 0.
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::435 => require(dialLen > 0 && _amounts.length == dialLen, "Invalid inputs");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::495 => } else if (latestVote.epoch == epoch && end > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::509 => if (dialData.cap > 0 && !dialData.disabled) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::592 => if (voterPreferences[_voter].lastSourcePoke > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::626 => require(_preferences[i].weight > 0, "Must give a dial some weight");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::668 => if (amount > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::672 => require(addTime > 0, "Caller must be staking contract");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::680 => } else if (lastSourcePoke > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::693 => } else if (lastSourcePoke > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/DelayedClaimableGovernor.sol::27 => require(_delay > 0, "Delay must be greater than zero");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::206 => if (oldScaledBalance > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::301 => _units > 0 && _units <= totalUnits,
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::250 => if (src != dst && amount > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::284 => if (pos > 0 && ckpts[pos - 1].fromBlock == block.number) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::239 => if (oldScaledBalance > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::295 => _units > 0 && _units <= _totalUnits,
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::239 => if (src != dst && amount > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::273 => if (pos > 0 && ckpts[pos - 1].fromBlock == block.number) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::173 => bool _exitCooldown = (_oldBalance.cooldownTimestamp > 0 &&
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::185 => _multiplier > 0 && _multiplier <= 50,
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::279 => require(len > 0, "No quest IDs");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::330 => require(len > 0, "No accounts");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::221 => (oldBalance.cooldownTimestamp > 0 &&
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::122 => require(len > 0, "No bAssets");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::205 => require(_inputQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::240 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::274 => require(_inputQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::297 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::331 => require(_inputQuantity > 0, "Invalid swap quantity");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::371 => require(_inputQuantity > 0, "Invalid swap quantity");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::417 => require(_mAssetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::464 => require(_mAssetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::513 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::514 => require(_maxMassetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::552 => require(_mAssetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::576 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::804 => require(mintAmount > 0, "Must collect something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::99 => if (_inputQuantities[i] > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::122 => require(mintOutput > 0, "Zero mAsset quantity");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::174 => require(swapOutput > 0, "Zero output quantity");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::227 => require(bAssetQuantity > 0, "Output == 0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::173 => if (bal > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::199 => require(len > 0, "Must migrate some bAssets");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::221 => if (lendingBal > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::225 => if (cache > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::241 => if (lendingBal > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::343 => require(_targetA > 0 && _targetA < MAX_A, "A target out of bounds");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::154 => require(len > 0, "No bAssets");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::237 => require(_inputQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::272 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::306 => require(_inputQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::329 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::363 => require(_inputQuantity > 0, "Invalid swap quantity");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::403 => require(_inputQuantity > 0, "Invalid swap quantity");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::449 => require(_mAssetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::496 => require(_mAssetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::545 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::546 => require(_maxMassetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::584 => require(_mAssetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::608 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::836 => require(mintAmount > 0, "Must collect something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::145 => require(len > 0, "No bAssets");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::228 => require(_inputQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::263 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::297 => require(_inputQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::320 => require(len > 0 && len == _inputs.length, "Input array mismatch");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::354 => require(_inputQuantity > 0, "Invalid swap quantity");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::394 => require(_inputQuantity > 0, "Invalid swap quantity");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::440 => require(_mAssetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::487 => require(_mAssetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::536 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::537 => require(_maxMassetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::575 => require(_mAssetQuantity > 0, "Qty==0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::599 => require(len > 0 && len == _outputs.length, "Invalid array input");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::827 => require(mintAmount > 0, "Must collect something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::95 => require(len > 0, "No keys provided");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::141 => require(timestamp > 0, "Proposed module not found");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::165 => require(len > 0, "Keys array empty");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::233 => require(proposedLockModules[_key] > 0, "Module lock request not found");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::285 => if (_timestamp > 0 && block.timestamp >= _timestamp + UPGRADE_DELAY)
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::105 => if (newRewardPerToken > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::140 => if (reward > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::268 => if (_reward > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::215 => require(_amount > 0, "Must deposit something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::225 => if (totalCredits > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::327 => require(_underlying > 0, "Must deposit something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::366 => require(_credits > 0, "Must withdraw something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::392 => require(_credits > 0, "Must withdraw something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::418 => require(_underlying > 0, "Must withdraw something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::474 => require(_amount > 0, "Must withdraw something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::686 => require(_data.totalCredits > 0, "Must have something to poke");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::705 => if (connectorBalance > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::273 => if (interestCollected > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::414 => if (interestCollected > 0 || newReward > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/upgradability/DelayedProxyAdmin.sol::130 => if (_timestamp > 0 && block.timestamp >= _timestamp + UPGRADE_DELAY)
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::63 => if (amount > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::134 => if (weighting > 0) {
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockMasset.sol::81 => require(outputQuantity > 0, "Output == 0");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::63 => require(_whitelisted.length > 0, "Empty whitelist array");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::133 => require(_amount > 0, "Must deposit something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::194 => require(_totalAmount > 0, "Must withdraw something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::221 => require(_amount > 0, "Must withdraw something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/rewards/MockRootChainManager.sol::29 => require(amount > 0, "RootChainManager: INVALID_AMOUNT");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsContract.sol::103 => require(_amount > 0, "Must withdraw something");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsManager.sol::26 => if (interestCollected > 0) {
```
#### Tools used
Manual code review

### 4th Bug
Use immutable for OpenZeppelin AccessControl's Roles Declarations

#### Impact
Issue Information: 
⚡️ Only valid for solidity versions <0.6.12 ⚡️

Access roles marked as constant results in computing the keccak256 operation each time the variable is used because assigned operations for constant variables are re-evaluated every time.

Changing the variables to immutable results in computing the hash only once on deployment, leading to gas savings.

Example
🤦 Bad:

bytes32 public constant GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");
🚀 Good:

bytes32 public immutable GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");

#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::21 => bytes32 public constant MANAGER_WITHDRAW_ROLE = keccak256("Collateral_managerWithdraw(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::22 => bytes32 public constant SET_MANAGER_ROLE = keccak256("Collateral_setManager(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::23 => bytes32 public constant SET_DEPOSIT_FEE_ROLE = keccak256("Collateral_setDepositFee(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::24 => bytes32 public constant SET_WITHDRAW_FEE_ROLE = keccak256("Collateral_setWithdrawFee(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::25 => bytes32 public constant SET_DEPOSIT_HOOK_ROLE = keccak256("Collateral_setDepositHook(ICollateralHook)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::26 => bytes32 public constant SET_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setWithdrawHook(ICollateralHook)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::27 => bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::17 => bytes32 public constant SET_COLLATERAL_ROLE = keccak256("DepositHook_setCollateral(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::18 => bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("DepositHook_setDepositRecord(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::19 => bytes32 public constant SET_DEPOSITS_ALLOWED_ROLE = keccak256("DepositHook_setDepositsAllowed(bool)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::20 => bytes32 public constant SET_ACCOUNT_LIST_ROLE = keccak256("DepositHook_setAccountList(IAccountList)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::21 => bytes32 public constant SET_REQUIRED_SCORE_ROLE = keccak256("DepositHook_setRequiredScore(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::22 => bytes32 public constant SET_COLLECTION_SCORES_ROLE = keccak256("DepositHook_setCollectionScores(IERC721[],uint256[])");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::23 => bytes32 public constant REMOVE_COLLECTIONS_ROLE = keccak256("DepositHook_removeCollections(IERC721[])");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::24 => bytes32 public constant SET_TREASURY_ROLE = keccak256("DepositHook_setTreasury(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::25 => bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("DepositHook_setTokenSender(ITokenSender)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::14 => bytes32 public constant SET_GLOBAL_NET_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setGlobalNetDepositCap(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::15 => bytes32 public constant SET_USER_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setUserDepositCap(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::16 => bytes32 public constant SET_ALLOWED_HOOK_ROLE = keccak256("DepositRecord_setAllowedHook(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::13 => bytes32 public constant SET_COLLATERAL_ROLE = keccak256("ManagerWithdrawHook_setCollateral(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::14 => bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("ManagerWithdrawHook_setDepositRecord(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::15 => bytes32 public constant SET_MIN_RESERVE_PERCENTAGE_ROLE = keccak256("ManagerWithdrawHook_setMinReservePercentage(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::26 => bytes32 _salt = keccak256(abi.encodePacked(_longToken, _shortToken));
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::26 => bytes32 public constant SET_PRICE_ROLE = keccak256("TokenSender_setPrice(IUintValue)");
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::27 => bytes32 public constant SET_PRICE_MULTIPLIER_ROLE = keccak256("TokenSender_setPriceMultiplier(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::28 => bytes32 public constant SET_SCALED_PRICE_LOWER_BOUND_ROLE = keccak256("TokenSender_setScaledPriceLowerBound(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::29 => bytes32 public constant SET_ALLOWED_MSG_SENDERS_ROLE = keccak256("TokenSender_setAllowedMsgSenders(IAccountList)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::22 => bytes32 public constant SET_COLLATERAL_ROLE = keccak256("WithdrawHook_setCollateral(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::23 => bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("WithdrawHook_setDepositRecord(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::24 => bytes32 public constant SET_WITHDRAWALS_ALLOWED_ROLE = keccak256("WithdrawHook_setWithdrawalsAllowed(bool)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::25 => bytes32 public constant SET_GLOBAL_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setGlobalPeriodLength(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::26 => bytes32 public constant SET_USER_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setUserPeriodLength(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::27 => bytes32 public constant SET_GLOBAL_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setGlobalWithdrawLimitPerPeriod(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::28 => bytes32 public constant SET_USER_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setUserWithdrawLimitPerPeriod(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::29 => bytes32 public constant SET_TREASURY_ROLE = keccak256("WithdrawHook_setTreasury(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::30 => bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("WithdrawHook_setTokenSender(ITokenSender)");
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol::80 => * @dev `longShortHash` is a keccak256 hash of the long token address and
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::271 => * @param _signature Signature from the verified _questSigner, containing keccak hash of account & ids
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::321 => * @param _signature Signature from the verified _questMaster, containing keccak hash of id and accounts
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/deps/SignatureVerifier.sol::57 => return keccak256(abi.encodePacked(account, ids));
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/deps/SignatureVerifier.sol::65 => return keccak256(abi.encodePacked(id, accounts));
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/deps/SignatureVerifier.sol::74 => keccak256(
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/StakingRewardsDistribution.sol::38 => bytes32 _leaf = keccak256(abi.encodePacked(_account, _amount));
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/StakingRewardsDistribution.sol::63 => return uint256(keccak256(abi.encodePacked(_user, _periodNumber)));
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/GovernedMinterRole.sol::21 => bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::10 => * @dev    keccak256() values are hardcoded to avoid re-evaluation of the constants at runtime.
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::15 => // keccak256("Governance");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::18 => //keccak256("Staking");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::21 => //keccak256("ProxyAdmin");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::27 => // keccak256("OracleHub");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::30 => // keccak256("Manager");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::33 => //keccak256("Recollateraliser");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::36 => //keccak256("MetaToken");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::39 => // keccak256("SavingsManager");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::42 => // keccak256("Liquidator");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::45 => // keccak256("InterestValidator");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ModuleKeys.sol::48 => // keccak256("Keeper");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/upgradability/DelayedProxyAdmin.sol::159 => // bytes4(keccak256("admin()")) == 0xf851a440
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/upgradability/DelayedProxyAdmin.sol::177 => // bytes4(keccak256("implementation()")) == 0x5c60da1b
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegrationWithToken.sol::22 => address liquidator = nexus.getModule(keccak256("Liquidator"));
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockRewardToken.sol::19 => address liquidator = nexus.getModule(keccak256("Liquidator"));
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenesisPoints.sol::50 => bytes32 _leaf = keccak256(abi.encodePacked(_msgSender(), _amount));
```
#### Tools used
Manual code review

### 5th Bug
Long Revert Strings

#### Impact
Issue Information: 
Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

Example
🤦 Bad:

require(condition, "UniswapV3: The reentrancy guard. A transaction cannot re-enter the pool mid-swap");
🚀 Good (with shorter string):

// TODO: Provide link to a reference of error codes
require(condition, "LOK");
🚀 Good (with custom errors):

/// @notice A transaction cannot re-enter the pool mid-swap.
error NoReentrancy();

// ...

if (!condition) {
    revert NoReentrancy();
}

#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::4 => import "prepo-shared-contracts/contracts/interfaces/IAccountList.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/AccountList.sol::5 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::5 => import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-ERC20PermitUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::6 => import "@openzeppelin/contracts-upgradeable/security/ReentrancyGuardUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::7 => import "prepo-shared-contracts/contracts/SafeAccessControlEnumerableUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::21 => bytes32 public constant MANAGER_WITHDRAW_ROLE = keccak256("Collateral_managerWithdraw(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::23 => bytes32 public constant SET_DEPOSIT_FEE_ROLE = keccak256("Collateral_setDepositFee(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::24 => bytes32 public constant SET_WITHDRAW_FEE_ROLE = keccak256("Collateral_setWithdrawFee(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::25 => bytes32 public constant SET_DEPOSIT_HOOK_ROLE = keccak256("Collateral_setDepositHook(ICollateralHook)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::26 => bytes32 public constant SET_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setWithdrawHook(ICollateralHook)");
prepo-monorepo/apps/smart-contracts/core/contracts/Collateral.sol::27 => bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::6 => import "prepo-shared-contracts/contracts/AccountListCaller.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::7 => import "prepo-shared-contracts/contracts/NFTScoreRequirement.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::8 => import "prepo-shared-contracts/contracts/TokenSenderCaller.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::9 => import "prepo-shared-contracts/contracts/SafeAccessControlEnumerable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::10 => import "@openzeppelin/contracts/token/ERC721/IERC721.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::17 => bytes32 public constant SET_COLLATERAL_ROLE = keccak256("DepositHook_setCollateral(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::18 => bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("DepositHook_setDepositRecord(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::19 => bytes32 public constant SET_DEPOSITS_ALLOWED_ROLE = keccak256("DepositHook_setDepositsAllowed(bool)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::20 => bytes32 public constant SET_ACCOUNT_LIST_ROLE = keccak256("DepositHook_setAccountList(IAccountList)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::21 => bytes32 public constant SET_REQUIRED_SCORE_ROLE = keccak256("DepositHook_setRequiredScore(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::22 => bytes32 public constant SET_COLLECTION_SCORES_ROLE = keccak256("DepositHook_setCollectionScores(IERC721[],uint256[])");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::23 => bytes32 public constant REMOVE_COLLECTIONS_ROLE = keccak256("DepositHook_removeCollections(IERC721[])");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositHook.sol::25 => bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("DepositHook_setTokenSender(ITokenSender)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::5 => import "prepo-shared-contracts/contracts/SafeAccessControlEnumerable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::14 => bytes32 public constant SET_GLOBAL_NET_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setGlobalNetDepositCap(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::15 => bytes32 public constant SET_USER_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setUserDepositCap(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositRecord.sol::16 => bytes32 public constant SET_ALLOWED_HOOK_ROLE = keccak256("DepositRecord_setAllowedHook(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::4 => import "./interfaces/IDepositTradeHelper.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::5 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol::6 => import "@openzeppelin/contracts/token/ERC20/extensions/draft-IERC20Permit.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/LongShortToken.sol::4 => import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/LongShortToken.sol::5 => import "@openzeppelin/contracts/access/Ownable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/LongShortToken.sol::6 => import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::4 => import "./interfaces/IManagerWithdrawHook.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::5 => import "prepo-shared-contracts/contracts/SafeAccessControlEnumerable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::13 => bytes32 public constant SET_COLLATERAL_ROLE = keccak256("ManagerWithdrawHook_setCollateral(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::14 => bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("ManagerWithdrawHook_setDepositRecord(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol::15 => bytes32 public constant SET_MIN_RESERVE_PERCENTAGE_ROLE = keccak256("ManagerWithdrawHook_setMinReservePercentage(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::5 => import "prepo-shared-contracts/contracts/AllowedMsgSenders.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::6 => import "prepo-shared-contracts/contracts/AccountListCaller.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::7 => import "prepo-shared-contracts/contracts/interfaces/IAccountList.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/MintHook.sol::8 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::6 => import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::7 => import "@openzeppelin/contracts/access/Ownable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarket.sol::8 => import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::7 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::8 => import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::9 => import "@openzeppelin/contracts-upgradeable/security/ReentrancyGuardUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol::10 => import "./interfaces/IPrePOMarketFactory.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::6 => import "prepo-shared-contracts/contracts/AllowedMsgSenders.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::7 => import "prepo-shared-contracts/contracts/AccountListCaller.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::8 => import "prepo-shared-contracts/contracts/TokenSenderCaller.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/RedeemHook.sol::9 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::4 => import "prepo-shared-contracts/contracts/interfaces/ITokenSender.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::5 => import "prepo-shared-contracts/contracts/AllowedMsgSenders.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::6 => import "prepo-shared-contracts/contracts/WithdrawERC20.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::7 => import "prepo-shared-contracts/contracts/SafeAccessControlEnumerable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::8 => import "prepo-shared-contracts/contracts/interfaces/IUintValue.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::9 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::10 => import "@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::27 => bytes32 public constant SET_PRICE_MULTIPLIER_ROLE = keccak256("TokenSender_setPriceMultiplier(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::28 => bytes32 public constant SET_SCALED_PRICE_LOWER_BOUND_ROLE = keccak256("TokenSender_setScaledPriceLowerBound(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/TokenSender.sol::29 => bytes32 public constant SET_ALLOWED_MSG_SENDERS_ROLE = keccak256("TokenSender_setAllowedMsgSenders(IAccountList)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::6 => import "prepo-shared-contracts/contracts/TokenSenderCaller.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::7 => import "prepo-shared-contracts/contracts/SafeAccessControlEnumerable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::22 => bytes32 public constant SET_COLLATERAL_ROLE = keccak256("WithdrawHook_setCollateral(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::23 => bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("WithdrawHook_setDepositRecord(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::24 => bytes32 public constant SET_WITHDRAWALS_ALLOWED_ROLE = keccak256("WithdrawHook_setWithdrawalsAllowed(bool)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::25 => bytes32 public constant SET_GLOBAL_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setGlobalPeriodLength(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::26 => bytes32 public constant SET_USER_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setUserPeriodLength(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::27 => bytes32 public constant SET_GLOBAL_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setGlobalWithdrawLimitPerPeriod(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::28 => bytes32 public constant SET_USER_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setUserWithdrawLimitPerPeriod(uint256)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::29 => bytes32 public constant SET_TREASURY_ROLE = keccak256("WithdrawHook_setTreasury(address)");
prepo-monorepo/apps/smart-contracts/core/contracts/WithdrawHook.sol::30 => bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("WithdrawHook_setTokenSender(ITokenSender)");
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol::5 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol::6 => import "@openzeppelin/contracts-upgradeable/token/ERC20/IERC20Upgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/ICollateral.sol::7 => import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-IERC20PermitUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/IDepositTradeHelper.sol::6 => import "@uniswap/v3-periphery/contracts/interfaces/ISwapRouter.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/ILongShortToken.sol::4 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/interfaces/IPrePOMarketFactory.sol::30 => * "LONG "/"SHORT " are appended to respective names, "L_"/"S_" are
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::6 => import "@openzeppelin/contracts-upgradeable/token/ERC20/IERC20Upgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::7 => import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/IERC20MetadataUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::8 => import "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::9 => import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::166 => require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::207 => require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::234 => require(sender != address(0), "ERC20: transfer from the zero address");
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::235 => require(recipient != address(0), "ERC20: transfer to the zero address");
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::240 => require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::284 => require(account != address(0), "ERC20: burn from the zero address");
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::289 => require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::318 => require(owner != address(0), "ERC20: approve from the zero address");
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::319 => require(spender != address(0), "ERC20: approve to the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/mini-sales/AllowlistPurchaseHook.sol::4 => import "./interfaces/IAllowlistPurchaseHook.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/mini-sales/AllowlistPurchaseHook.sol::5 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/mini-sales/MiniSales.sol::5 => import "prepo-shared-contracts/contracts/WithdrawERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/mini-sales/MiniSalesFlag.sol::5 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/mini-sales/interfaces/IAllowlistPurchaseHook.sol::5 => import "prepo-shared-contracts/contracts/interfaces/IAccountList.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/mini-sales/interfaces/IMiniSales.sol::5 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/AccountList.sol::4 => import "prepo-shared-contracts/contracts/interfaces/IAccountList.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/AccountList.sol::5 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/BlocklistTransferHook.sol::4 => import "./interfaces/IBlocklistTransferHook.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/BlocklistTransferHook.sol::5 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/PPO.sol::40 => import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20BurnableUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/PPO.sol::42 => import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-ERC20PermitUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/PPO.sol::43 => import "prepo-shared-contracts/contracts/SafeOwnableUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/RestrictedTransferHook.sol::4 => import "./interfaces/IRestrictedTransferHook.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/interfaces/IBlocklistTransferHook.sol::5 => import "prepo-shared-contracts/contracts/interfaces/IAccountList.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/interfaces/IPPO.sol::5 => import "@openzeppelin/contracts-upgradeable/token/ERC20/IERC20Upgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/interfaces/IPPO.sol::6 => import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/draft-IERC20PermitUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo/interfaces/IRestrictedTransferHook.sol::5 => import "prepo-shared-contracts/contracts/interfaces/IAccountList.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::4 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::5 => import {IGovernanceHook} from "../governance/staking/interfaces/IGovernanceHook.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::6 => import {IRewardsDistributionRecipient} from "../interfaces/IRewardsDistributionRecipient.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::9 => import {Initializable} from "@openzeppelin/contracts/proxy/utils/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::10 => import {SafeERC20, IERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::4 => import {Initializable} from "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::5 => import {ContextUpgradeable} from "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::6 => import {SafeCastExtended} from "../../shared/SafeCastExtended.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::8 => import {HeadlessStakingRewards} from "../../rewards/staking/HeadlessStakingRewards.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::4 => import {MathUpgradeable} from "@openzeppelin/contracts-upgradeable/utils/math/MathUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::5 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::6 => import {ECDSAUpgradeable} from "@openzeppelin/contracts-upgradeable/utils/cryptography/ECDSAUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol::9 => import {Initializable} from "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::4 => import {Initializable} from "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::5 => import {ContextUpgradeable} from "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::6 => import {SafeCastExtended} from "../../shared/SafeCastExtended.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::8 => import {HeadlessStakingRewards} from "../../rewards/staking/HeadlessStakingRewards.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::9 => import {IAchievementsManager} from "./interfaces/IAchievementsManager.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::10 => import {ITimeMultiplierCalculator} from "./interfaces/ITimeMultiplierCalculator.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::11 => import "./deps/PPOGamifiedTokenStructs.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::4 => import {MathUpgradeable} from "@openzeppelin/contracts-upgradeable/utils/math/MathUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::5 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::6 => import {ECDSAUpgradeable} from "@openzeppelin/contracts-upgradeable/utils/cryptography/ECDSAUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol::9 => import {Initializable} from "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::7 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::8 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::9 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::11 => import {InitializableReentrancyGuard} from "../../shared/InitializableReentrancyGuard.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::12 => import "./deps/PPOGamifiedTokenStructs.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::4 => import {Initializable} from "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::5 => import {ContextUpgradeable} from "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::8 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::186 => "Quest multiplier too large > 1.5x"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol::252 => "All seasonal quests must have expired"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::7 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::8 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::9 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::11 => import {InitializableReentrancyGuard} from "../../shared/InitializableReentrancyGuard.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/SteppedTimeMultiplierV1.sol::4 => import "./interfaces/ITimeMultiplierCalculator.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/WithdrawalRights.sol::3 => import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/WithdrawalRights.sol::4 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/WithdrawalRights.sol::16 => constructor() ERC721("Staked PPO Withdrawal Rights", "stkPPO-WR") {}
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/IStakedToken.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IDisperse.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IEmissionsController.sol::5 => import {DialData} from "../emissions/EmissionsController.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IIncentivisedVotingLockup.sol::4 => import {IERC20WithCheckpointing} from "../shared/IERC20WithCheckpointing.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IRewardsDistributionRecipient.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/ISavingsContract.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IStakingRewardsWithPlatformToken.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IVotiumBribe.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::6 => import {Initializable} from "../shared/@openzeppelin-2.5/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::9 => import {InitializableReentrancyGuard} from "../shared/InitializableReentrancyGuard.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::14 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::5 => import {IPlatformIntegration} from "../interfaces/IPlatformIntegration.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::12 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::13 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::14 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::6 => import {IPlatformIntegration} from "../interfaces/IPlatformIntegration.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::7 => import {IInvariantValidator} from "../interfaces/IInvariantValidator.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::14 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::15 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::16 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::85 => "Token must have sufficient decimal places"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::337 => "Sufficient period of previous ramp has not elapsed"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::6 => import {Initializable} from "../../shared/@openzeppelin-2.5/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::7 => import {InitializableToken, IERC20} from "../../shared/InitializableToken.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::9 => import {InitializableReentrancyGuard} from "../../shared/InitializableReentrancyGuard.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::14 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::21 => import {IBasketManager} from "../../z_mocks/masset/migrate2/IBasketManager.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::22 => import {Basket, Basset} from "../../z_mocks/masset/migrate2/MassetStructsV1.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::23 => import {InitializableModuleV1} from "../../z_mocks/masset/migrate2/InitializableModuleV1.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1Migrator.sol::7 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1Migrator.sol::9 => import {IBasketManager} from "../../z_mocks/masset/migrate2/IBasketManager.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1Migrator.sol::10 => import {Basket, Basset} from "../../z_mocks/masset/migrate2/MassetStructsV1.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::6 => import {Initializable} from "../../shared/@openzeppelin-2.5/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::7 => import {InitializableToken, IERC20} from "../../shared/InitializableToken.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::9 => import {InitializableReentrancyGuard} from "../../shared/InitializableReentrancyGuard.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::14 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/mocks/MockAchievementsManager.sol::4 => import "../governance/staking/interfaces/IAchievementsManager.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/mocks/MockPPOGamifiedToken.sol::4 => import "../governance/staking/PPOGamifiedToken.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/mocks/MockPPOStaking.sol::4 => import "../governance/staking/PPOStaking.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/nexus/Nexus.sol::6 => import {DelayedClaimableGovernor} from "../governance/DelayedClaimableGovernor.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/InitializableRewardsDistributionRecipient.sol::5 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/InitializableRewardsDistributionRecipient.sol::6 => import {IRewardsDistributionRecipient} from "../interfaces/IRewardsDistributionRecipient.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::5 => import {InitializableRewardsDistributionRecipient} from "../InitializableRewardsDistributionRecipient.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::8 => import {ContextUpgradeable} from "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::11 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::12 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::13 => import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::14 => import {Initializable} from "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/HeadlessStakingRewards.sol::302 => "Cannot notify with more than a million units"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/PlatformTokenVendor.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/PlatformTokenVendorFactory.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/StakingRewardsDistribution.sol::4 => import "@openzeppelin/contracts/utils/cryptography/MerkleProof.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/StakingRewardsDistribution.sol::5 => import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/StakingRewardsDistribution.sol::6 => import "../../interfaces/IStakingRewardsDistribution.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/StakingRewardsDistribution.sol::7 => import "../../governance/staking/interfaces/IPPOStaking.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/rewards/staking/StakingRewardsDistribution.sol::8 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::5 => import {ISavingsManager} from "../interfaces/ISavingsManager.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::8 => import {ISavingsContractV3} from "../interfaces/ISavingsContract.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::13 => import {Initializable} from "../shared/@openzeppelin-2.5/Initializable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::16 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::265 => "Can only use this method before streaming begins"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::6 => import {ISavingsContractV2} from "../interfaces/ISavingsContract.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::9 => import {IRevenueRecipient} from "../interfaces/IRevenueRecipient.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::10 => import {ISavingsManager} from "../interfaces/ISavingsManager.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::14 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::15 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::370 => "Must have a valid savings contract"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin/InstantProxyAdmin.sol::5 => import "@openzeppelin/contracts/proxy/transparent/ProxyAdmin.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::5 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::129 => "ERC20: transfer amount exceeds allowance"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::183 => "ERC20: decreased allowance below zero"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::209 => require(sender != address(0), "ERC20: transfer from the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::210 => require(recipient != address(0), "ERC20: transfer to the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::215 => require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::253 => require(account != address(0), "ERC20: burn from the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::258 => require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::283 => require(owner != address(0), "ERC20: approve from the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::284 => require(spender != address(0), "ERC20: approve to the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/Initializable.sol::21 => "Contract instance has already been initialized"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ERC1155Mintable.sol::4 => import "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ERC1155Mintable.sol::5 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ERC721Mintable.sol::4 => import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/ERC721Mintable.sol::5 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/GovernedMinterRole.sol::6 => import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/GovernedMinterRole.sol::7 => import {AccessControl} from "@openzeppelin/contracts/access/AccessControl.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/GovernedMinterRole.sol::30 => "MinterRole: caller does not have the Minter role"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/InitializableERC20Detailed.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/MassetHelpers.sol::4 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/MassetHelpers.sol::5 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/MetaToken.sol::5 => import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/MetaToken.sol::6 => import {ERC20Burnable} from "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::34 => "SafeCast: value doesn't fit in 224 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::52 => "SafeCast: value doesn't fit in 128 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::70 => "SafeCast: value doesn't fit in 96 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::88 => "SafeCast: value doesn't fit in 88 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::106 => "SafeCast: value doesn't fit in 64 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::124 => "SafeCast: value doesn't fit in 32 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::142 => "SafeCast: value doesn't fit in 16 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::158 => require(value <= type(uint8).max, "SafeCast: value doesn't fit in 8 bits");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::190 => "SafeCast: value doesn't fit in 128 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::211 => "SafeCast: value doesn't fit in 64 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::232 => "SafeCast: value doesn't fit in 32 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::253 => "SafeCast: value doesn't fit in 16 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::274 => "SafeCast: value doesn't fit in 8 bits"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/SafeCastExtended.sol::290 => "SafeCast: value doesn't fit in an int256"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/YieldValidator.sol::75 => "Interest protected from inflating past maxAPY"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/YieldValidator.sol::80 => "Interest protected from inflating past 10 Bps"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/upgradability/DelayedProxyAdmin.sol::5 => import {TransparentUpgradeableProxy} from "@openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/upgradability/Proxies.sol::4 => // import { InitializableAdminUpgradeabilityProxy } from "../shared/@openzeppelin-2.5/upgrades/InitializableAdminUpgradeabilityProxy.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/upgradability/Proxies.sol::5 => import {TransparentUpgradeableProxy} from "@openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::4 => import {IGovernanceHook} from "../../governance/staking/interfaces/IGovernanceHook.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::5 => import {GamifiedVotingToken} from "../../governance/staking/GamifiedVotingToken.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::6 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::36 => "Must be whitelisted staking contract"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::52 => "Cannot add existing contract while users have preferences"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockStakedTokenWithPrice.sol::5 => import {StakedToken} from "../../governance/staking/StakedToken.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/stakedTokenWrapper.sol::4 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/stakedTokenWrapper.sol::5 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/stakedTokenWrapper.sol::6 => import {StakedToken} from "../../governance/staking/StakedToken.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockAave.sol::7 => import {IERC20, ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockMasset.sol::5 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::5 => import {IPlatformIntegration} from "../../interfaces/IPlatformIntegration.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::11 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::12 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegration.sol::199 => require(_amount == _totalAmount, "Cache inactive for assets with fee");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockPlatformIntegrationWithToken.sol::23 => require(liquidator != address(0), "Liquidator address cannot be zero");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockRewardToken.sol::6 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/MockRewardToken.sol::20 => require(liquidator != address(0), "Liquidator address cannot be zero");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/rewards/MockRewardsDistributionRecipient.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/rewards/MockRewardsDistributionRecipient.sol::5 => import {IRewardsRecipientWithPlatformToken} from "../../interfaces/IRewardsDistributionRecipient.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/rewards/MockRewardsDistributionRecipient.sol::6 => import {IRewardsDistributionRecipient} from "../../interfaces/IRewardsDistributionRecipient.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/rewards/MockRootChainManager.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/rewards/MockRootChainManager.sol::5 => import {IRootChainManager} from "../../interfaces/IRootChainManager.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/rewards/MockRootChainManager.sol::24 => "RootChainManager: INVALID_ROOT_TOKEN"
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsContract.sol::4 => import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsManager.sol::5 => import {ISavingsContractV1} from "../../interfaces/ISavingsContract.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsManager.sol::6 => import {IRevenueRecipient} from "../../interfaces/IRevenueRecipient.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsManager.sol::19 => require(save != address(0), "Must have a valid savings contract");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockSavingsManager.sol::41 => require(save != address(0), "Must have a valid savings contract");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/MockStakingContract.sol::3 => import {IGovernanceHook} from "../../governance/staking/interfaces/IGovernanceHook.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/connectors/MockConnector.sol::4 => import {IERC20, ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/savings/connectors/MockConnector.sol::5 => import {IConnector} from "../../../savings/peripheral/IConnector.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockERC20.sol::4 => import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockERC20WithFee.sol::5 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockERC20WithFee.sol::243 => require(sender != address(0), "ERC20: transfer from the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockERC20WithFee.sol::244 => require(recipient != address(0), "ERC20: transfer to the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockERC20WithFee.sol::285 => require(account != address(0), "ERC20: burn from the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockERC20WithFee.sol::310 => require(owner != address(0), "ERC20: approve from the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockERC20WithFee.sol::311 => require(spender != address(0), "ERC20: approve to the zero address");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockInitializableToken.sol::4 => import {ERC205} from "../../shared/@openzeppelin-2.5/ERC205.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockInitializableToken.sol::5 => import {InitializableERC20Detailed} from "../../shared/InitializableERC20Detailed.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenPass.sol::4 => import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenPass.sol::5 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenesisPoints.sol::4 => import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenesisPoints.sol::5 => import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenesisPoints.sol::6 => import "@openzeppelin/contracts/utils/cryptography/MerkleProof.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenesisPoints.sol::7 => import "./interfaces/IPregenesisPoints.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/pregenesis/PregenesisPoints.sol::8 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/PurchaseHook.sol::6 => import "prepo-shared-contracts/contracts/SafeOwnable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::4 => import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::5 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::6 => import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::7 => import "@openzeppelin/contracts/token/ERC721/IERC721.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::10 => import "prepo-shared-contracts/contracts/Pausable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::11 => import "prepo-shared-contracts/contracts/WithdrawERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::12 => import "prepo-shared-contracts/contracts/WithdrawERC721.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/token-shop/TokenShop.sol::13 => import "prepo-shared-contracts/contracts/WithdrawERC1155.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/vesting/Vesting.sol::4 => import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/vesting/Vesting.sol::6 => import "prepo-shared-contracts/contracts/Pausable.sol";
prepo-monorepo/apps/smart-contracts/token/contracts/vesting/Vesting.sol::7 => import "prepo-shared-contracts/contracts/WithdrawERC20.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/AccountListCaller.sol::5 => import "./interfaces/IAccountListCaller.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/AllowedMsgSenders.sol::4 => import "./interfaces/IAllowedMsgSenders.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/ERC1155Mintable.sol::4 => import "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/ERC20Mintable.sol::4 => import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/ERC721Mintable.sol::4 => import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::4 => import "./interfaces/INFTScoreRequirement.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::5 => import "@openzeppelin/contracts/utils/structs/EnumerableMap.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol::23 => require(collections.length == scores.length, "collections.length != scores.length");
prepo-monorepo/packages/prepo-shared-contracts/contracts/SafeAccessControlEnumerableCaller.sol::4 => import "./interfaces/ISafeAccessControlEnumerable.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/SafeAccessControlEnumerableCaller.sol::5 => import "./interfaces/ISafeAccessControlEnumerableCaller.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/SafeAccessControlEnumerableCaller.sol::6 => import "@openzeppelin/contracts/access/IAccessControl.sol";
prepo-monorepo/::4 => import "@openzeppelin/contracts-upgradeable/access/AccessControlEnumerableUpgradeable.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/SafeOwnableCaller.sol::5 => import "./interfaces/ISafeOwnableCaller.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/TokenSenderCaller.sol::4 => import "./interfaces/ITokenSenderCaller.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/WithdrawERC1155.sol::4 => import "./interfaces/IWithdrawERC1155.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/WithdrawERC1155.sol::6 => import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/WithdrawERC1155.sol::7 => import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/WithdrawERC721.sol::6 => import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/WithdrawERC721.sol::7 => import "@openzeppelin/contracts/token/ERC721/IERC721.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/INFTScoreRequirement.sol::4 => import "@openzeppelin/contracts/token/ERC721/IERC721.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/ISafeAccessControlEnumerable.sol::4 => import "@openzeppelin/contracts/access/IAccessControlEnumerable.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/ISafeAccessControlEnumerableUpgradeable.sol::4 => import "@openzeppelin/contracts-upgradeable/access/IAccessControlEnumerableUpgradeable.sol";
prepo-monorepo/packages/prepo-shared-contracts/contracts/interfaces/ITokenSender.sol::5 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
```
#### Tools used
Manual code review

### 6th Bug
Use Shift Right/Left instead of Division/Multiplication if possible

#### Impact
Issue Information: 
A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

Example
🤦 Bad:

uint256 b = a / 2;
uint256 c = a / 4;
uint256 d = a * 8;
🚀 Good:

uint256 b = a >> 1;
uint256 c = a >> 2;
uint256 d = a << 3;

#### Findings:
```
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::21 => * https://forum.zeppelin.solutions/t/how-to-implement-erc20-supply-mechanisms/226[How
prepo-monorepo/apps/smart-contracts/core/contracts/openzeppelin/ERC20UpgradeableRenameable.sol::85 => * be displayed to a user as `5.05` (`505 / 10 ** 2`).
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::186 => // 2.0 - Add each of the dials
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::242 => ((B * (x**2)) / 1e24) + // e.g.  168479942061125e3 * (3205128205 ^ 2) / 1e24 =  1730768635433
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::472 => // 2.0 - Calculate the total amount of dial votes ignoring any disabled dials
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::529 => // 4.0 - Calculate the distribution amounts for each dial
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::561 => // 2.0 - Reset the balance in storage back to 0
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::567 => // 4.0 - Notify the dial of the new rewards if configured to
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::621 => // 2.0 - Log new preferences
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol::635 => // 2.1 - In the likely scenario less than 16 preferences are given, add a breaker with max uint
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::274 => // 2. Take the opportunity to set weighted timestamp, if it changes
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::305 => // 2. Set weighted timestamp and enter cooldown
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::318 => // 4. Update scaled balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::337 => // 2. Set weighted timestamp and exit cooldown
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::347 => // 4. Update scaled balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::367 => // 2. Set weighted timestamp, if it changes
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::402 => // 2. Exit cooldown if necessary
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::423 => (totalRaw + (_rawAmount / 2));
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::473 => // 2. If we are exiting cooldown, reset the balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::484 => (totalRaw - (_rawAmount / 8));
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol::495 => // 4. Update scaled balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::268 => // 2. Take the opportunity to set weighted timestamp, if it changes
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::299 => // 2. Set weighted timestamp and enter cooldown
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::312 => // 4. Update scaled balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::331 => // 2. Set weighted timestamp and exit cooldown
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::341 => // 4. Update scaled balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::361 => // 2. Set weighted timestamp, if it changes
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::400 => // 2. Exit cooldown if necessary
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::425 => (_totalRaw + (_rawAmount / 2));
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::477 => // 2. If we are exiting cooldown, reset the balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::488 => (_totalRaw - (_rawAmount / 8));
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::499 => // 4. Update scaled balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol::585 => // 2. Update scaled balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::180 => // 2. Settle the stake by depositing the STAKED_TOKEN and minting voting power
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::223 => // 2. Return a proportionate amount of tokens, based on the collateralisation ratio
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::242 => // 2. Get current balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::259 => // 4. Exit cooldown if the user has specified, or if they have withdrawn everything
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol::328 => // 2. Take slashing percentage
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::215 => // 2. Deal with cooldown
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::271 => // 2. Return a proportionate amount of tokens, based on the collateralisation ratio
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::290 => // 2. Get current balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::307 => // 4. Exit cooldown if the user has specified, or if they have withdrawn everything
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol::376 => // 2. Take slashing percentage
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IStakingRewardsWithPlatformToken.sol::36 => * 2. Swaps the underlying mAsset tokens for fAsset tokens in a Feeder Pool.
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::377 => // 2. If a bAsset swap, calculate the validity, output and fee
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::824 => *      _cacheSize * totalSupply / 2 under normal circumstances.
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/Masset.sol::859 => require(_min <= 1e18 / (data.bAssetData.length * 2), "Min weight oob");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::175 => //4. Settle the swap
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::176 => //4.1. Decrease output bal
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::230 => // 2.0. Transfer the Bassets to the recipient
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::430 => // 2 - Deposit X if necessary
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::431 => // 2.1 - Deposit if xfer fees
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::441 => // 2.2 - Else Deposit X if Cache > %
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::448 => uint256 delta = cacheBal - (relativeMaxCache / 2);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::490 => // 2.1 - If balance b in cache, simply withdraw
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::498 => // 2.2 - Else reset the cache to X, or as far as possible
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::553 => // 2. Get value of reserves according to invariant
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::560 => // 4. Finalise mint
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::582 => // 2. Get value of reserves according to invariant
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::595 => // 4. Finalise mint
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::622 => // 2. Get value of reserves according to invariant
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::628 => // 4. Calc total mAsset q
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::684 => // 2. Get value of reserves according to invariant
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::698 => // 4. Check for max weight
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::737 => // 2. Get value of reserves according to invariant
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::748 => // 4. Get new value of reserves according to invariant
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::798 => // 2. Total minted is the difference between values, with respect to total supply
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::944 => y = (Root.sqrt((b**2) + (4 * c)) + b) / 2 + 1;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetLogic.sol::947 => y = (Root.sqrt((b**2) + (4 * c)) - b) / 2 + 1;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::207 => // 2. Withdraw everything from the old platform integration
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::219 => // 2.1. Withdraw from the lending market
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::224 => // 2.2. Withdraw from the cache, if any
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::234 => // 4. Deposit everything into the new
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::236 => // 4.1. Deposit all bAsset
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/MassetManager.sol::244 => // 4.2. Check balances
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::409 => // 2. If a bAsset swap, calculate the validity, output and fee
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::856 => *      _cacheSize * totalSupply / 2 under normal circumstances.
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1.sol::891 => require(_min <= 1e18 / (data.bAssetData.length * 2), "Min weight oob");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV1Migrator.sol::58 => maxScaledVaultBalance = (maxScaledVaultBalance * 2501) / 10000;
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::400 => // 2. If a bAsset swap, calculate the validity, output and fee
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::847 => *      _cacheSize * totalSupply / 2 under normal circumstances.
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/masset/versions/MV2.sol::882 => require(_min <= 1e18 / (data.bAssetData.length * 2), "Min weight oob");
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::700 => // 2. Check and verify new connector balance
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::738 => // 4i. Refresh exchange rate and emit event
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsContract.sol::743 => // 4ii. Refresh exchange rate and emit event
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::383 => // 2. Update all the time stamps
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/savings/SavingsManager.sol::434 => // 4. Distribute the interest
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/Context.sol::25 => this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/@openzeppelin-2.5/ERC205.sol::15 => * https://forum.zeppelin.solutions/t/how-to-implement-erc20-supply-mechanisms/226[How
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/InitializableERC20Detailed.sol::49 => * be displayed to a user as `5,05` (`505 / 10 ** 2`).
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/MassetHelpers.sol::31 => IERC20(_asset).safeApprove(_spender, 2**256 - 1);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/StableMath.sol::37 => * @return Ratio scale unit (1e8 or 1 * 10**8)
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/shared/YieldValidator.sol::67 => // e.g. 0.01% (1e14 * 1e18) / 2.74..e15 = 3.65e16 or 3.65% apr
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::89 => // 2. Fetch old bitmap and reduce all based on old preference
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/MockEmissionController.sol::132 => weighting = uint8(bitmap >> (i * 8));
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/governance/stakedTokenWrapper.sol::20 => rewardsToken.safeApprove(_stakedToken, 2**256 - 1);
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/migrate2/InitializableModuleV1.sol::14 => bytes32 private KEY_GOVERNANCE_DEPRICATED; // 2.x
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/masset/migrate2/InitializableModuleV1.sol::21 => bytes32 private KEY_RECOLLATERALISER_DEPRICATED; // 2.x
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockERC20WithFee.sol::15 => * mechanisms](https://forum.zeppelin.solutions/t/how-to-implement-erc20-supply-mechanisms/226).*
prepo-monorepo/apps/smart-contracts/token/contracts/ppo-staking/z_mocks/shared/MockERC20WithFee.sol::80 => * be displayed to a user as `5,05` (`505 / 10 ** 2`).
```
#### Tools used
Manual code review