### 1. Auction can be created without public key.
SizeSealed.createAuction [do not check](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L55-L110) if public key was provided by seller. 

If it was not provided that means that users can’t bid on it as they do not know public key that should be used for encryption.

### 2. Do not allow users to bid if they provide different public key than auction.params.pubKey
When seller creates auction, then he provides `auction.params.pubKey` param. This public key should be used by all bidders.

Function `bid` also has input param `pubKey` which means the public key that was used to sign bid.
If `pubKey != auction.params.pubKey` the function should revert as this means that this bid will never be executed.


