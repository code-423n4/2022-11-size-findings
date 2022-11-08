# Report
## Gas Optimizations ##
### [G-1]: The increment in for loop post condition can be made unchecked
**Context:**

1. ```for (uint256 i; i < bidIndices.length; i++) {``` [L244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244) 
1. ```for (uint256 i; i < seenBidMap.length - 1; i++) {``` [L302](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302) 

**Description:**

[This](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked) can save 30-40 gas per loop iteration.

**Recommendation:**

Example how to fix. Change:
```
for (uint256 i = 0; i < orders.length; ++i) {
   // Do the thing
}
```

To:
```
for (uint256 i = 0; i < orders.length;) {
   // Do the thing
   unchecked { ++i; }
}
```

### [G-2]: Place subtractions where the operands can't underflow in unchecked {} block
**Context:**

1. ```uint128 unsoldBase = data.totalBaseAmount - data.filledBase;``` [L319](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L319) 
1. ```baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart``` [L65](https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L65) (currentTime - vestingStart)

**Description:**

About 40 gas can be saved by using an unchecked {} block if an underflow isn't possible because of a previous require() or if-statement.


### [G-3]: Use calldata instead of memory
**Context:**

1. ```function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)``` [L217](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217) 
1. ```function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {``` [L25](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L25)
1. ```function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)``` [L37](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L37)
1. ```function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)``` [L51](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L51)
1. ```function hashPoint(Point memory point) internal pure returns (bytes32) {``` [L60](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L60)

**Description:**

If a reference type function parameter is read-only, it is recommended to use calldata instead of memory because this provides significant gas savings. Since Solidity v0.6.9, memory and calldata are allowed in all functions regardless of their visibility type (ie external, public, etc).
