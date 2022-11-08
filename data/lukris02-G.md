# Gas Optimizations Report for SIZE contest
## Overview
During the audit, 4 gas issues were found.  
Total savings ~450+.

№ | Title | Instance Count | Saved
--- | --- | --- | ---
G-1 | Use ```calldata``` instead of ```memory for``` read-only arguments | 5 | 300
G-2 | Use unchecked blocks for incrementing i | 2 | 70
G-3 | Use unchecked blocks for subtractions where underflow is impossible | 2 | 70
G-4 | Elements that are smaller than 32 bytes (256 bits) may increase gas usage | 11 |  

## Gas Optimizations Findings(4)
### G-1. Use ```calldata``` instead of ```memory for``` read-only arguments
##### Description
Since Solidity v0.6.9, memory and calldata are allowed in all functions regardless of their visibility type (See "Calldata Variables" section [here](https://blog.soliditylang.org/2020/06/05/Solidity-069-release-announcement/)).  
When function arguments should not be modified, it is cheaper to use calldata.

##### Instances
- [```function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217) 
- [```function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {```](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L25) 
- [```function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)```](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L37) 
- [```function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)```](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L51)
- [```function hashPoint(Point memory point) internal pure returns (bytes32) {```](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L60) 

##### Recommendation
Consider using calldata where possible.
##### Saved
This saves at least 60 gas per iteration.  
So, ~60*5 = 300
#
### G-2. Use unchecked blocks for incrementing i
##### Description
In Solidity 0.8+, there’s a default overflow and underflow check on unsigned integers. In the loops, "i" will not overflow because the loop will run out of gas before that.
##### Instances
- [```for (uint256 i; i < bidIndices.length; i++) {```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244) 
- [```for (uint256 i; i < seenBidMap.length - 1; i++) {```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302) 

##### Recommendation
Change:
```
for (uint256 i; i < n; ++i) {
 // ...
}
```
to:
```
for (uint256 i; i < n;) { 
 // ...
 unchecked { ++i; }
}
```
##### Saved
This saves ~30-40 gas per iteration.  
So, ~35*2 = 70
#
### G-3. Use unchecked blocks for subtractions where underflow is impossible
##### Description
In Solidity 0.8+, there’s a default overflow and underflow check on unsigned integers. When an overflow or underflow isn’t possible (after require or if-statement), some gas can be saved by using [unchecked blocks](https://docs.soliditylang.org/en/v0.8.17/control-structures.html#checked-or-unchecked-arithmetic).
##### Instances
- [```uint128 unsoldBase = data.totalBaseAmount - data.filledBase;```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L319) 
- [```currentTime - vestingStart```](https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L65) 

##### Saved
This saves ~35.  
So, ~35*2 = 70
#
### G-4. Elements that are smaller than 32 bytes (256 bits) may increase gas usage
##### Description
According to [docs](https://docs.soliditylang.org/en/v0.8.16/internals/layout_in_storage.html#layout-of-state-variables-in-storage), when using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
##### Instances
- [```uint128 totalBaseAmount;```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L205) 
- [```uint128 filledBase;```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L206) 
- [```uint32 startTimestamp;```](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L56) 
- [```uint32 endTimestamp;```](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L57) 
- [```uint32 vestingStartTimestamp;```](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L58) 
- [```uint32 vestingEndTimestamp;```](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L59) 
- [```uint128 cliffPercent;```](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L60) 
- [```uint128 lowestBase;```](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L65) 
- [```uint128 lowestQuote;```](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L66) 
- [```uint128 totalBaseAmount;```](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L80) 
- [```uint128 minimumBidQuote;```](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L81) 

##### Recommendation
Consider using a larger size where needed.