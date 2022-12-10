## Impact
Protocol protections for limited amount of users can be bypassed

The protocol sets the following limits to accounts that can participate in the protocol at launch:
1. Users in the account list
2. Users that own NFT collections supported by the protocol. These NFTs are given to users/investors that have participated in previous activities with PrePo.

These protections are in place because PrePo has put constrained capital limits.

These protections can be bypassed if NFTs supported by `NFTScoreRequirement` are transferable (not soulbound) .

Currently, according to the sponsor all supported tokens are Galxe NFT tokens that are soulbound. That is why I have assigned this issue as low.

The NFT contracts themselves have `setTransferable` function which can can be called by the owner(PrePO) and change the token to be transferable.

Additionally, if in the future the protocol will set additional NFTs that are transferable to be supported by the `NFTScoreRequirement` then the bug will be exploitable

## Proof of Concept
(Assuming NFTs are transferable)

Users are allowed to participate if they own an NFT supported by the `NFTScoreRequirement` contract. 
Specifically, `getAccountScore` will be called to iterate over the supported NFT collections to identify the account balances of the collections.
https://github.com/prepo-io/prepo-monorepo/blob/3541bc704ab185a969f300e96e2f744a572a3640/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol#L60
```
  function getAccountScore(address account) public view virtual override returns (uint256) {
    uint256 score;
    uint256 numCollections = _collectionToScore.length();
    for (uint256 i = 0; i < numCollections; ) {
      (address collection, uint256 collectionScore) = _collectionToScore.at(i);
      score += IERC721(collection).balanceOf(account) > 0 ? collectionScore : 0;
      unchecked {
        ++i;
      }
    }
    return score;
  }
```

If any user that owned an NFT supported in `NFTScoreRequirement` contract sold that NFT in NFT markets or swap pools, other users can flash loan the NFT and participate in the protocol.

Same transaction attack flow:
1. Flash loan and swap/buy NFT in AMM products such as sudoswap or nft20
2. Deposit collateral to PrePo (deposit will succeed because account holds the NFT temporarily)
3. Return NFT to the flash loan

Unlimited amount of depositors can use the above flow and use the same NFTs to participate in PrePo

## Tools Used

VS Code

## Recommended Mitigation Steps

Options:
1. Validate that NFTScoreRequirement only supports soulbound NFT token collections.
2. Require that the users deposit their NFT tokens. The protocol can mint them an alternative token.