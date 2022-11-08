# SIZE Contest Gas Optimization Report

## Summary

The following gas optimization issues were found during the code audit:

1. Use `calldata` instead of `memory` (8 instances)
2. Cache `<array>.length` (2 instances)
3. Use `unchecked{}` to suppress overflow/underflow check (2 instances)
4. Use `++i`/`--i` instead of `i++`/`i--` (2 instances)
5. Use `abi.encodePacked()` instead of `abi.encode()` (3 instances)
6. Use shift right/left instead of division/multiplication if possible (2 instances)

Total 6 instances of 19 issues.

## 1. Use `calldata` instead of `memory` (8 instances)

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for loop to copy each index of the `calldata` to the `memory` index. This overhead can be optimized by using `calldata` directly.

```solidity
src/SizeSealed.sol::217 => function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)

src/SizeSealed.sol::474 => function getTimings(uint256 auctionId) external view returns (Timings memory timings) {

src/SizeSealed.sol::478 => function getAuctionData(uint256 auctionId) external view returns (AuctionData memory data) {

src/util/ECCMath.sol::18 => function publicKey(uint256 privateKey) internal view returns (Point memory) {

src/util/ECCMath.sol::25 => function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {

src/util/ECCMath.sol::37 => function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)

src/util/ECCMath.sol::51 => function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)

src/util/ECCMath.sol::60 => function hashPoint(Point memory point) internal pure returns (bytes32) {
```

## 2. Cache `<array>.length` (2 instances)

If `<array>.length` is used as for loop termination condition, then the `.length` method will be called in each iteration. Caching it in a local variable can save gas.

```solidity
src/SizeSealed.sol::244 => for (uint256 i; i < bidIndices.length; i++) {

src/SizeSealed.sol::302 => for (uint256 i; i < seenBidMap.length - 1; i++) {
```

## 3. Use `unchecked{}` to suppress overflow/underflow check (2 instances)

Starting from version 0.8.0, Solidity does overflow/underflow checks by default. It is a good feature to prevent vulnerabilities but it has a significant overhead, especially when used in for loop. When using uint256/int256, it is extremely hard to trigger overflow, so it makes sense to skip these checks. To suppress the overflow/underflow checks, use `unchecked {}`. For increment situations, just use `unchecked {}` directly; for decrement situations, add a `require()` statement before decrementing to prevent underflow.

```solidity
src/SizeSealed.sol::244 => for (uint256 i; i < bidIndices.length; i++) {

src/SizeSealed.sol::302 => for (uint256 i; i < seenBidMap.length - 1; i++) {
```

## 4. Use `++i`/`--i` instead of `i++`/`i--` (2 instances)

Using `++i`/`--i` saves 6 gas per loop.

```solidity
src/SizeSealed.sol::244 => for (uint256 i; i < bidIndices.length; i++) {

src/SizeSealed.sol::302 => for (uint256 i; i < seenBidMap.length - 1; i++) {
```

## 5. Use `abi.encodePacked()` instead of `abi.encode()` (3 instances)

`abi.encodePacked()` is more efficient than `abi.encode()`.

```solidity
src/SizeSealed.sol::467 => return keccak256(abi.encode(message));

src/util/ECCMath.sol::26 => bytes memory data = abi.encode(point, scalar);

src/util/ECCMath.sol::61 => return keccak256(abi.encode(point));
```

## 6. Use shift right/left instead of division/multiplication if possible (2 instances)

A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left. While the `DIV` opcode uses 5 gas, the `SHR` opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

```solidity
src/SizeSealed.sol::241 => uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);

src/SizeSealed.sol::249 => uint256 bitmapIndex = bidIndex / 256;
```
