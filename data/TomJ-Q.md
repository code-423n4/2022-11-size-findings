## Table of Contents
### Low Risk Issues
- Missing Zero Address Check 

### Non-critical Issues
- Event is Missing Indexed Fields
- Define Magic Numbers to Constant

&ensp;
## Low Risk Issues
### Missing Zero Address Check 

#### Issue
createAuction function of SizeSealed.sol has NO Zero address check for `auctionParams.quoteToken`.
Therefore it is possible for seller to create auction with 0-address `quoteToken` which will create invalid auction
since no one can bit against it.

#### PoC
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L55-L110

#### Mitigation
Add Zero addresss check for `auctionParams.quoteToken` in createAuction function.
```solidity
        if (auctionParams.quoteToken == address(0)) {
            revert InvalidState();
        }
```

&ensp;
## Non-critical Issues
### Event is Missing Indexed Fields

#### Issue
Each event should have 3 indexed fields if there are 3 or more fields.

#### PoC
Total of 8 instances found.
```
ISizeSealed.sol:
 97:    event AuctionCreated(
 98:         uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
 99:    );
...
101:    event AuctionCancelled(uint256 auctionId);
...
103:    event Bid(
104:        address sender,
105:        uint256 auctionId,
106:        uint256 bidIndex,
107:        uint128 quoteAmount,
108:        bytes32 commitment,
109:        ECCMath.Point pubKey,
110:        bytes32 encryptedMessage,
111:        bytes encryptedPrivateKey
112:    );
...
114:    event BidCancelled(uint256 auctionId, uint256 bidIndex);
...
116:    event RevealedKey(uint256 auctionId, uint256 privateKey);
...
118:    event AuctionFinalized(uint256 auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);
...
120:    event BidRefund(uint256 auctionId, uint256 bidIndex);
...
122:    event Withdrawal(uint256 auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);
```

#### Mitigation
Add up to 3 indexed fields when possible.

&ensp;
### Define Magic Numbers to Constant

#### Issue
It is best practice to define magic numbers to constant rather than just using as a magic number.
This improves code readability and maintainability. 

#### PoC
1. Magic Number: 1e18
```solidity
./CommonTokenMath.sol:60:            uint256 cliffAmount = FixedPointMathLib.mulDivDown(baseAmount, cliffPercent, 1e18);
./SizeSealed.sol:72:        if (timings.cliffPercent > 1e18) {
```

2. Magic Number: 1000
```solidity
./SizeSealed.sol:157:        if (bidIndex >= 1000) {
```

#### Mitigation
Define magic numbers to constant.

&ensp;