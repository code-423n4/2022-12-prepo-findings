## QA
---

### Layout Order

- The best-practices for layout within a contract is the following order: state variables, events, modifiers, constructor and functions.

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol

```solidity
// modifiers declared after constructor and functions
111:  modifier onlyRecollateralisationModule() {
122:  modifier onlyBeforeRecollateralisation() {
138:  modifier assertNotContract() {
```


https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol

```solidity
// modifier declared after constructor and functions
68:  modifier questMasterOrGovernor() {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOStaking.sol

```solidity
// modifiers declared after constructor and functions
107:  modifier onlyRecollateralisationModule() {
118:  modifier onlyBeforeRecollateralisation() {
134:  modifier assertNotContract() {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol

```solidity

// events coming before state variables
33:  event TimeMultiplierCalculatorChange(address newCalculator);
34:  event MaxMultiplierChange(uint256 newMultiplier);

// modifier coming after functions
87:  modifier onlyAchievementsManager() {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/Governable.sol

```solidity
// modifier coming after constructor and functions
41:  modifier onlyGovernor() {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol

```solidity
// modifier declared after constructor and function
87:  modifier onlyQuestManager() {
```

---

### Function Visibility

- Order of Functions: Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. Functions should be grouped according to their visibility and ordered: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Within a grouping, place the view and pure functions last.

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/WithdrawalRights.sol

```solidity
// public functions should come before external ones
31:  function tokenURI(uint256 _tokenId)
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol

```solidity
  Different visibility functions are mixing throughout the code
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol

```solidity
  Different visibility functions are mixing throughout the code
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol

```solidity
// public function called after a private one
196:  function delegate(address _delegatee) public virtual {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol

```solidity
  Different visibility functions are mixing throughout the code
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol

```solidity
  Different visibility functions are mixing throughout the code
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/Governable.sol

```solidity
// external function coming after public functions
58:  function changeGovernor(address _newGovernor) external virtual onlyGovernor {
```

---

### natSpec missing

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IVotiumBribe.sol

```solidity
6:  interface IVotiumBribe {

7:  function depositBribe(
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IVotes.sol

```solidity
4:    interface IVotes {

5:  function getVotes(address account) external view returns (uint256);
```


https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IStakingRewardsDistribution.sol

```solidity
4:    interface IStakingRewardsDistribution {

5:  event RewardClaim(

10:  event RootUpdate(bytes32 indexed newRoot, uint256 newPeriodNumber);

12:  function setPPOStaking(address newPPOStaking) external;

14:  function setMerkleTreeRoot(bytes32 newRoot) external;

16:  function claim(

22:  function getPPOStaking() external view returns (address);

24:  function getMerkleTreeRoot() external view returns (bytes32);

26:  function getPeriodNumber() external view returns (uint256);

28:  function hasClaimed(address user) external view returns (bool);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/ISavingsContract.sol

```solidity
6:   interface ISavingsContractV1 {

7:  function depositInterest(uint256 _amount) external;

9:  function depositSavings(uint256 _amount)

13:  function redeem(uint256 _amount) external returns (uint256 massetReturned);

15:  function exchangeRate() external view returns (uint256);

17:  function creditBalances(address) external view returns (uint256);

20:   interface ISavingsContractV2 {

22:  function redeem(uint256 _amount) external returns (uint256 massetReturned);

24:  function creditBalances(address) external view returns (uint256); // V1 & V2 (use balanceOf)

28:  function depositInterest(uint256 _amount) external; // V1 & V2

30:  function depositSavings(uint256 _amount)

34:  function depositSavings(uint256 _amount, address _beneficiary)

38:  function redeemCredits(uint256 _amount)

42:  function redeemUnderlying(uint256 _amount)

46:  function exchangeRate() external view returns (uint256); // V1 & V2

48:  function balanceOfUnderlying(address _user)

53:  function underlyingToCredits(uint256 _underlying)

58:  function creditsToUnderlying(uint256 _credits)

63:  function underlying() external view returns (IERC20 underlyingMasset); // V2

66:   interface ISavingsContractV3 {

68:  function redeem(uint256 _amount) external returns (uint256 massetReturned);

70:  function creditBalances(address) external view returns (uint256); // V1 & V2 (use balanceOf)

74:  function depositInterest(uint256 _amount) external; // V1 & V2

76:  function depositSavings(uint256 _amount)

80:  function depositSavings(uint256 _amount, address _beneficiary)

84:  function redeemCredits(uint256 _amount)

88:  function redeemUnderlying(uint256 _amount)

92:  function exchangeRate() external view returns (uint256); // V1 & V2

94:  function balanceOfUnderlying(address _user)

99:  function underlyingToCredits(uint256 _underlying)

104:  function creditsToUnderlying(uint256 _credits)

109:  function underlying() external view returns (IERC20 underlyingMasset); // V2

113:  function redeemAndUnwrap(

129:  function depositSavings(
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IRootChainManager.sol

```solidity
4:  interface IRootChainManager {

5:    function depositFor(

12:   interface IStateReceiver {

13:  function onStateReceive(uint256 id, bytes calldata data) external;

16:  interface IChildToken {

17:  function deposit(address user, bytes calldata depositData) external;

19:  function withdraw(uint256 amount) external;
```


https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IRewardsDistributionRecipient.sol

```solidity
6:  interface IRewardsDistributionRecipient {

7:  function notifyRewardAmount(uint256 reward) external;

9:  function getRewardToken() external view returns (IERC20);

12: interface IRewardsRecipientWithPlatformToken {

13:  function notifyRewardAmount(uint256 reward) external;

15:  function getRewardToken() external view returns (IERC20);

17:  function getPlatformToken() external view returns (IERC20);
```


https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IRevenueRecipient.sol

```solidity
4:  interface IRevenueRecipient {

9:  function depositToPool(
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/INexus.sol

```solidity
9:  function governor() external view returns (address);

11:  function getModule(bytes32 key) external view returns (address);

13:  function proposeModule(bytes32 _key, address _addr) external;

15:  function cancelProposedModule(bytes32 _key) external;

17:  function acceptProposedModule(bytes32 _key) external;

19:  function acceptProposedModules(bytes32[] calldata _keys) external;

21:  function requestLockModule(bytes32 _key) external;

23:  function cancelLockModule(bytes32 _key) external;

25:  function lockModule(bytes32 _key) external;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IMasset.sol

```solidity
7:  abstract contract IMasset {

9:  function mint(

16:  function mintMulti(

23:  function getMintOutput(address _input, uint256 _inputQuantity)

29:  function getMintMultiOutput(

35:  function swap(

43:  function getSwapOutput(

50:  function redeem(

57:  function redeemMasset(

63:  function redeemExactBassets(

70:  function getRedeemOutput(address _output, uint256 _mAssetQuantity)

76:  function getRedeemExactBassetsOutput(

82:  function getBasket() external view virtual returns (bool, bool);

84:  function getBasset(address _token)

90:  function getBassets()

96:  function bAssetIndexes(address) external view virtual returns (uint8);

98:  function getPrice() external view virtual returns (uint256 price, uint256 k);

101:  function collectInterest()

106:  function collectPlatformInterest()

112:  function setCacheSize(uint256 _cacheSize) external virtual;

114:  function setFees(uint256 _swapFee, uint256 _redemptionFee) external virtual;

116:  function setTransferFeesFlag(address _bAsset, bool _flag) external virtual;

118:  function migrateBassets(address[] calldata _bAssets, address _newIntegration)
```


https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IInvariantValidator.sol

```solidity
6:  abstract contract IInvariantValidator {

8:  function computeMint(

15:  function computeMintMulti(

23:  function computeSwap(

33:  function computeRedeem(

40:  function computeRedeemExact(

47:  function computePrice(
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IIncentivisedVotingLockup.sol

```solidity
6:  abstract contract IIncentivisedVotingLockup is IERC20WithCheckpointing {

7:  function getLastUserPoint(address _addr)

17:  function createLock(uint256 _value, uint256 _unlockTime) external virtual;

19:  function withdraw() external virtual;

21:  function increaseLockAmount(uint256 _value) external virtual;

23:  function increaseLockLength(uint256 _unlockTime) external virtual;

25:  function eject(address _user) external virtual;

27:  function expireContract() external virtual;

29:  function claimReward() public virtual;

31:  function earned(address _account) public view virtual returns (uint256);

33:  function exit() external virtual;
```


https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IFeederPool.sol

```solidity
7:  abstract contract IFeederPool {

9:  function mint(

16:  function mintMulti(

23:  function getMintOutput(address _input, uint256 _inputQuantity)

29:  function getMintMultiOutput(

35:  function swap(

43:  function getSwapOutput(

50:  function redeem(

57:  function redeemProportionately(

63:  function redeemExactBassets(

70:  function getRedeemOutput(address _output, uint256 _fpTokenQuantity)

76:  function getRedeemExactBassetsOutput(

82:  function mAsset() external view virtual returns (address);

84:  function getPrice() public view virtual returns (uint256 price, uint256 k);

86:  function getConfig()

92:  function getBasset(address _token)

98:  function getBassets()

105:  function collectPlatformInterest()

110:  function collectPendingFees() external virtual;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IFAssetRedemptionPriceGetter.sol

```solidity
4:  interface IFAssetRedemptionPriceGetter {

5:  function snappedRedemptionPrice() external view returns (uint256);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IEmissionsController.sol

```solidity
12:  function getDialRecipient(uint256 dialId)

16:  function donate(uint256[] memory _dialIds, uint256[] memory _amounts)

19:  function stakingContracts(uint256 dialId) external returns (address);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IEjector.sol

```solidity
9:  function ejectMany(address[] calldata _users) external;

11:  function votingLockup() external view returns (address);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/interfaces/IDisperse.sol

```soliidty
6:  interface IDisperse {

7:  function disperseTokenSimple(
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/emissions/EmissionsController.sol

```solidity
54:   struct TopLevelConfig {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/deps/SignatureVerifier.sol

```solidity
27:   library SignatureVerifier {

28:  function verify(

40:  function verify(

52:  function getMessageHash(address account, uint256[] memory ids)

60:  function getMessageHash(uint256 id, address[] memory accounts)

68:  function getEthSignedMessageHash(bytes32 messageHash)

79:  function recoverSigner(

88:  function splitSignature(bytes memory signature)


```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/ITimeMultiplierCalculator.sol

```solidity
4:  interface ITimeMultiplierCalculator {

5:  function calculate(uint256 _timestamp) external view returns (uint256);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/IStakedToken.sol

```solidity
7:  interface IStakedToken {

9:  function COOLDOWN_SECONDS() external view returns (uint256);

11:  function UNSTAKE_WINDOW() external view returns (uint256);

13:  function STAKED_TOKEN() external view returns (IERC20);

15:  function getRewardToken() external view returns (address);

17:  function pendingAdditionalReward() external view returns (uint256);

19:  function whitelistedWrappers(address) external view returns (bool);

21:  function balanceData(address _account)

26:  function balanceOf(address _account) external view returns (uint256);

28:  function rawBalanceOf(address _account)

33:  function calcRedemptionFeeRate(uint32 _weightedTimestamp)

38:  function safetyData()

43:  function delegates(address account) external view returns (address);

45:  function getPastTotalSupply(uint256 blockNumber)

50:  function getPastVotes(address account, uint256 blockNumber)

55:  function getVotes(address account) external view returns (uint256);

58:  function applyQuestMultiplier(address _account, uint8 _newMultiplier)

62:  function whitelistWrapper(address _wrapper) external;

64:  function blackListWrapper(address _wrapper) external;

66:  function changeSlashingPercentage(uint256 _newRate) external;

68:  function emergencyRecollateralisation() external;

70:  function setGovernanceHook(address _newHook) external;

73:  function stake(uint256 _amount) external;

75:  function stake(uint256 _amount, address _delegatee) external;

77:  function stake(uint256 _amount, bool _exitCooldown) external;

79:  function withdraw(

86:  function delegate(address delegatee) external;

88:  function startCooldown(uint256 _units) external;

90:  function endCooldown() external;

92:  function reviewTimestamp(address _account) external;

94:  function claimReward() external;

96:  function claimReward(address _to) external;

99:  function createLock(uint256 _value, uint256) external;

101:  function exit() external;

103:  function increaseLockAmount(uint256 _value) external;

105:  function increaseLockLength(uint256) external;

```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/IQuestManager.sol

```solidity
6:  interface IQuestManager {

7:  event QuestAdded(

15:  event QuestCompleteQuests(address indexed user, uint256[] ids);

16:  event QuestCompleteUsers(uint256 indexed questId, address[] accounts);

17:  event QuestExpired(uint16 indexed id);

18:  event QuestMaster(address oldQuestMaster, address newQuestMaster);

19:  event QuestSeasonEnded();

20:  event QuestSigner(address oldQuestSigner, address newQuestSigner);

21:  event StakedTokenAdded(address stakedToken);

24:  function balanceData(address _account)

29:  function getQuest(uint256 _id) external view returns (Quest memory);

31:  function hasCompleted(address _account, uint256 _id)

36:  function questMaster() external view returns (address);

38:  function seasonEpoch() external view returns (uint32);

41:  function addQuest(

47:  function addStakedToken(address _stakedToken) external;

49:  function expireQuest(uint16 _id) external;

51:  function setQuestMaster(address _newQuestMaster) external;

53:  function setQuestSigner(address _newQuestSigner) external;

55:  function startNewQuestSeason() external;

58:  function completeUserQuests(

64:  function completeQuestUsers(

70:  function checkForSeasonFinish(address _account)

```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/IPPOStaking.sol

```solidity
4:  interface IPPOStaking {

5:  function stake(address recipient, uint256 amount) external;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/IGovernanceHook.sol

```solidity
4:  interface IGovernanceHook {

5:  function moveVotingPowerHook(
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/IBVault.sol

```solidity
4:  struct ExitPoolRequest {

11:  interface IBVault {

12:  function exitPool(

19:  function getPoolTokens(bytes32 poolId)
```


https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/IAchievementsManager.sol

```solidity
10:  function checkForSeasonFinish(address account) external returns (int64);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/WithdrawalRights.sol

```solidity
16:  constructor() ERC721("Staked PPO Withdrawal Rights", "stkPPO-WR") {}

18:  function setURI(string memory _newURI) external onlyOwner {

22:  function setPPOStaking(address _newPPOStaking) external onlyOwner {

26:  function mint(address _to) external onlyPPOStaking {

31:  function tokenURI(uint256 _tokenId)

40:  function getPPOStaking() external view returns (address) {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/StakedToken.sol

```solidity
127:  function _onlyBeforeRecollateralisation() internal view {

143:  function _assertNotContract() internal view {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/QuestManager.sol

```solidity
73:  function _questMasterOrGovernor() internal view {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedVotingToken.sol

```solidity
57:  constructor(

// function is also not implemented and not set as `virtual` in case of being overriden
63:  function __PPOGamifiedVotingToken_init() internal {}

// incomplete natspec
68:  function setGovernanceHook(address _newHook) external onlyGovernor {

234:  function _moveVotingPower(

264:  function _writeCheckpoint(

285:  function _add(uint256 a, uint256 b) private pure returns (uint256) {

289:  function _subtract(uint256 a, uint256 b) private pure returns (uint256) {



```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/PPOGamifiedToken.sol

```solidity
95:  function setTimeMultiplierCalculator(address _newCalculator)

103:  function setMaxMultiplier(uint256 _newMaxMultiplier) external onlyGovernor {

112:  function name() public view override returns (string memory) {

116:  function symbol() public view override returns (string memory) {

189:  function getTimeMultiplierCalculator()

197:  function getMaxMultiplier() external view returns (uint256) {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedVotingToken.sol

```solidity
26:  struct Checkpoint {

57:  constructor(

// incomplete natSpec
69:  function setGovernanceHook(address _newHook) external onlyGovernor {

245:  function _moveVotingPower(

275:  function _writeCheckpoint(

296:  function _add(uint256 a, uint256 b) private pure returns (uint256) {

300:  function _subtract(uint256 a, uint256 b) private pure returns (uint256) {


```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol

```solidity
96:  function name() public view override returns (string memory) {

100:  function symbol() public view override returns (string memory) {

251:  function _getPriceCoeff() internal virtual returns (uint256) {

613:  function bytes32ToString(bytes32 _bytes32)
```


https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/ClaimableGovernor.sol

```solidity
36:  constructor(address _governorAddr) {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/mini-sales/MiniSalesFlag.sol
```solidity
10:  function setSaleStarted(bool _newSaleStarted) external override onlyOwner {

14:  function hasSaleStarted() external view override returns (bool) {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/mini-sales/interfaces/IMiniSalesFlag.sol
```solidity
5:  function setSaleStarted(bool newSaleStarted) external;

7:  function hasSaleStarted() external view returns (bool);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/mini-sales/interfaces/IMiniSalesFlag.sol
```solidity
5:   function setSaleStarted(bool newSaleStarted) external;

7:  function hasSaleStarted() external view returns (bool);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/mini-sales/AllowlistPurchaseHook.sol
```solidity
12:  function hook(

22:  function setAllowlist(IAccountList _newAllowlist)

31:  function getAllowlist() external view override returns (IAccountList) {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/mini-sales/MiniSales.sol

```solidity
14:  constructor(

24:  function purchase(

52:  function setPrice(uint256 _newPrice) external override onlyOwner {

57:  function setPurchaseHook(IPurchaseHook _newPurchaseHook)

66:  function getSaleToken() external view override returns (IERC20) {

70:  function getPaymentToken() external view override returns (IERC20) {

74:  function getPrice() external view override returns (uint256) {

78:  function getPurchaseHook() external view override returns (IPurchaseHook) {

82:  function getSaleForPayment(uint256 payment)
```

---

### State variable and function names

- Variables should be named according to their specifications
  - private and internal `variables` should preppend with `underline`
  - private and internal `functions` should preppend with `underline`
  - constant state variables should be UPPER_CASE

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/GamifiedToken.sol

```solidity
37:  uint8 public constant override decimals = 18;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/mini-sales/MiniSales.sol

```solidity
8:  IERC20 private immutable saleToken;

9:  IERC20 private immutable paymentToken;

10:  uint256 private immutable saleTokenDenominator;

11:  uint256 private price;

12:  IPurchaseHook private purchaseHook;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/mini-sales/MiniSalesFlag.sol

```solidity
8:  bool private saleStarted;
```

---

### Version

- Pragma versions should be standardized and avoid floating pragma `( ^ )`

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/deps/SignatureVerifier.sol

```solidity
// do not conform with the rest of the repository
25: pragma solidity ^0.8.0;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/ILockedERC20.sol

```solidity
// do not conform with the rest of the repository
3:  pragma solidity ^0.8.0;
```


https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/staking/interfaces/IBVault.sol

```solidity
// do not conform with the rest of the repository
2:  pragma solidity ^0.8.0;
```


---

### Initialized variables

- Variables are default initialized with 0 for `uint / int`, 0x0 for `address` and false for `[bool]`

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/DelayedClaimableGovernor.sol

```solidity
16:   uint256 public delay = 0;
17:   uint256 public requestTime = 0;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/token/contracts/ppo-staking/governance/ClaimableGovernor.sol

```solidity
26:  address public proposedGovernor = address(0);
```
