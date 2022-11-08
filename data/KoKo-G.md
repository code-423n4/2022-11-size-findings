# GAS ISSUES FOR [2022-11-SIZE](https://github.com/code-423n4/2022-11-size/tree/main/<file-path>)

## [G-01] `uncheck` the `i++`/`i--` in for loops since there's no way to overflow/underflow

./src/SizeSealed.sol
```
L244:  for (uint256 i; i < bidIndices.length; i++) {

L302:  for (uint256 i; i < seenBidMap.length - 1; i++) {
```

## [G-02] Use `++i` instead of `i++`

./src/SizeSealed.sol
```
L244:  for (uint256 i; i < bidIndices.length; i++) {

L302:  for (uint256 i; i < seenBidMap.length - 1; i++) {
```

## [G-03] A < B + 1 is cheaper than A <= B

./src/util/CommonTokenMath.sol
```
L56:   } else if (currentTime <= vestingStart) {
```

./src/SizeSealed.sol
```
L35:   } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {

L63:   if (timings.startTimestamp >= timings.endTimestamp) {

L157:  if (bidIndex >= 1000) {

L270:  if (quotePerBase >= data.previousQuotePerBase) {

L425:  if (block.timestamp >= a.timings.endTimestamp) {
```
