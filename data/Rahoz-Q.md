
## L. MISSING NATSPEC
Code should include NatSpec
`AuctionParameters.merkleRoot`
`EncryptedBid`
`AuctionData`
### Proof of Concept
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L82
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L40-L48
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L63-L68

## N. FUNCTION ORDER
Functions should be ordered following the Solidity conventions.
Link: https://docs.soliditylang.org/en/v0.8.15/style-guide.html#order-of-functions
### Proof of Concept
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

## N. INTERFACE FILES SHOULD USE FLOATING COMPILER VERSIONS
Floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.
### Proof of Concept
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L2
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L2
### Recommended Mitigation Steps
Should use floating versions

## N. EVENT IS MISSING INDEXED FIELDS
Index event fields make the field more quickly accessible to off-chain tools that parse events. 
### Proof of Concept
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L97-L122
### Recommended Mitigation Steps
We should add indexed for auctionId in event `AuctionCreated`, `AuctionCancelled`, `Bid`,`BidCancelled`, `RevealedKey`, `AuctionFinalized`, `BidRefund`,`Withdrawal`, 
