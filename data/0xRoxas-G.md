# Gas Report
## Optimizations found [4]

### [G-01] Use Bit Shift Operators for MUL/DIV/MOD of Powers of 2 [ⓘ](https://github.com/devanshbatham/Solidity-Gas-Optimization-Tips#1--use-of-bit-shift-operators)

#### Findings [4]:

*/src/SizeSealed.sol*
Line(s): [241](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L241), [249](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L249), [251](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L251), [309](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L309)
```solidity
241:	uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);
249:	uint256 bitmapIndex = bidIndex / 256;
251:	uint256 indexBit = 1 << (bidIndex % 256);
309:	if (seenBidMap[seenBidMap.length - 1] != (1 << (bidIndices.length % 256)) - 1) {
```

**Suggested Change:**
```solidity
241:	uint256[] memory seenBidMap = new uint256[]((bidIndices.length >> 8)+1);
249:	uint256 bitmapIndex = bidIndex >> 8;
251:	uint256 indexBit = 1 << (bidIndex & 0xff);
309:	if (seenBidMap[seenBidMap.length - 1] != (1 << (bidIndices.length & 0xff)) - 1) {
```


### [G-02] Public Constants Can Be Made Private to Save Gas

Public constants can be made private to save on gas. If a contract needs to value they can simply verify it from the source.

#### Findings [2]:

*/src/util/ECCMath.sol*
Line(s): [8](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L8), [9](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L9)
```solidity
8:	uint256 internal constant GX = 1;
9:	uint256 internal constant GY = 2;
```

### [G-03] Use nested if instead of && [ⓘ](https://github.com/devanshbatham/Solidity-Gas-Optimization-Tips#6--use-nested-if-and-avoid-multiple-check-combinations)

#### Findings [3]:

*/src/util/ECCMath.sol*
Line(s): [27](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L27)
```solidity
27:	if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);
```

*/src/SizeSealed.sol*
Line(s): [187](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L187), [258](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L258)
```solidity
187:	if (pubKey.x != a.params.pubKey.x || pubKey.y != a.params.pubKey.y || (pubKey.x == 1 && pubKey.y == 1)) {
258:	if (sharedPoint.x == 1 && sharedPoint.y == 1) continue;
```

### [G-04] Using `unchecked` When Arithmetic Cannot Overflow Saves Gas

There are 2 occurances where arithmetic is performed that cannot overflow / underflow based on a previous require. Adding an `unchecked` brace around these occurances will save gas (Solidity will not force an underflow / overflow).

**NOTE:** Findings show the check before each line that allows the second line to be `unchecked`.  Only the line following the check should be unchecked (NOT the check itself).

```solidity
unchecked {
	//Code
}
```

#### Findings [2]:

*/src/SizeSealed.sol*
Line(s): [289-290](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L289-L290)

```solidity
289:	if (data.filledBase + baseAmount > data.totalBaseAmount) {
290:	baseAmount = data.totalBaseAmount - data.filledBase;
```

*/src/util/CommonTokenMath.sol*
Line(s): [54](https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L54) / [56](https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L54) / [65](https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L54)

```solidity
54:	if (currentTime > vestingEnd) {
56:	} else if (currentTime <= vestingStart) {
65:	baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart
```