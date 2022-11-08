# GAS OPTIMIZATIONS REPORT

## SIZE

### Use `unchecked` for the counter in `for` loops

Since it is compared to a target value (usually the length of an array) there is no way the counter overflows

[SizeSealed.sol](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol):

```solidity
line#L244: for (uint256 i; i < bidIndices.length; i++) {

line#L302: for (uint256 i; i < seenBidMap.length - 1; i++) {
```

### Use `calldata` where possible instead of `memory` to save gas

[SizeSealed.sol](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol):

```solidity
line#L217: function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
```

[ECCMath.sol](https://github.com/code-423n4/2022-11-size/tree/main/src/util/ECCMath.sol):

```solidity

line#L37:  function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)

line#L51:  function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)

line#L60:  function hashPoint(Point memory point) internal pure returns (bytes32) {
```

### `++i` costs less gas than `i++` (same for `--i`/`i--`)

Prefix increments are cheaper than postfix increments

[SizeSealed.sol](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol):

```solidity
line#L244: for (uint256 i; i < bidIndices.length; i++) {

line#L302: for (uint256 i; i < seenBidMap.length - 1; i++) {
```

### Use `uint256` instead of the smaller uints where possible

The word-size in ethereum is 32 bytes which means that every variables which is sized with less bytes will cost more to operate with

[SizeSealed.sol](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol):

```solidity
line#L124: uint128 quoteAmount,

line#L196: (uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote) =

line#L206: uint128 filledBase;

line#L217: function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)

line#L319: uint128 unsoldBase = data.totalBaseAmount - data.filledBase;

line#L365: uint128 baseAmount = b.filledBaseAmount;

line#L451: function tokensAvailableForWithdrawal(uint256 auctionId, uint128 baseAmount)

line#L454: returns (uint128 tokensAvailable)
```

[CommonTokenMath.sol](https://github.com/code-423n4/2022-11-size/tree/main/src/util/CommonTokenMath.sol):

```solidity
line#L48:  uint32 vestingStart,

line#L49:  uint32 vestingEnd,

line#L51:  uint128 cliffPercent,

line#L52:  uint128 baseAmount
```

### `x < y + 1` is cheaper than `x <= y`

[SizeSealed.sol](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol):

```solidity
line#L35:  } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {

line#L60:  if (timings.endTimestamp <= block.timestamp) {

line#L157: if (bidIndex >= 1000) {

line#L270: if (quotePerBase >= data.previousQuotePerBase) {

line#L426: if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {
```

[CommonTokenMath.sol](https://github.com/code-423n4/2022-11-size/tree/main/src/util/CommonTokenMath.sol):

```solidity
line#L56:  } else if (currentTime <= vestingStart) {
```
