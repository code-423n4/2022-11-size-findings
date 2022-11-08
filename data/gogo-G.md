# 2022-11-SIZE

## Gas Optimizations Report

### The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

This change would save up to 6 gas per instance/loop.

_There are **2** instances of this issue:_

```solidity
File: src/SizeSealed.sol

244:  for (uint256 i; i < bidIndices.length; i++) {

302:  for (uint256 i; i < seenBidMap.length - 1; i++) {
```

https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol

### `++i`/`i++` should be `unchecked{++I}`/`unchecked{I++}` in `for`-loops

When an increment or any arithmetic operation is not possible to overflow it should be placed in `unchecked{}` block. \This is because of the default compiler overflow and underflow safety checks since Solidity version 0.8.0. \In for-loops it saves around 30-40 gas **per loop**

_There are **2** instances of this issue:_

```solidity
File: src/SizeSealed.sol

244:  for (uint256 i; i < bidIndices.length; i++) {

302:  for (uint256 i; i < seenBidMap.length - 1; i++) {
```

https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol

### Use `calldata` instead of `memory` for function parameters

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **5** instances of this issue:_

```solidity
File: src/SizeSealed.sol

217:  function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
```

https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol

```solidity
File: src/util/ECCMath.sol

25:   function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {

37:   function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)

51:   function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)

60:   function hashPoint(Point memory point) internal pure returns (bytes32) {
```

https://github.com/code-423n4/2022-11-size/tree/main/src/util/ECCMath.sol

### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `>` and `=`. Using strict comparison operators hence saves gas

_There are **8** instances of this issue:_

```solidity
File: src/SizeSealed.sol

35:    } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {

60:    if (timings.endTimestamp <= block.timestamp) {

63:    if (timings.startTimestamp >= timings.endTimestamp) {

157:   if (bidIndex >= 1000) {

270:       if (quotePerBase >= data.previousQuotePerBase) {

425:   if (block.timestamp >= a.timings.endTimestamp) {

426:       if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {
```

https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol

```solidity
File: src/util/CommonTokenMath.sol

56:    } else if (currentTime <= vestingStart) {
```

https://github.com/code-423n4/2022-11-size/tree/main/src/util/CommonTokenMath.sol
