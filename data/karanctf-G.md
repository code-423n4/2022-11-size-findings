
## 1. cache `.length` in loops
**Summary:**    Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the array length in the stack saves around 3 gas per iteration.

```solidity
SizeSealed.sol:155:        uint256 bidIndex = a.bids.length;
SizeSealed.sol:195:        if (finalizeData.length != 0) {
SizeSealed.sol:227:        if (bidIndices.length != a.bids.length) {
SizeSealed.sol:241:        uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);
SizeSealed.sol:244:        for (uint256 i; i < bidIndices.length; i++) {
SizeSealed.sol:302:        for (uint256 i; i < seenBidMap.length - 1; i++) {
SizeSealed.sol:309:        if (seenBidMap[seenBidMap.length - 1] != (1 << (bidIndices.length % 256)) - 1) {
```


## 2. `<x> += <y>` costs more gas than `<x> = <x> + <y>` 
```solidity
SizeSealed.sol:294:            data.filledBase += baseAmount;
SizeSealed.sol:373:        b.baseWithdrawn += baseTokensAvailable;
```
## 3. Use `calldata` instead of `memory` when used only for reading
```solidity
util/ECCMath.sol:25:    function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {util/ECCMath.sol:37:    function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)
util/ECCMath.sol:51:    function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)
util/ECCMath.sol:60:    function hashPoint(Point memory point) internal pure returns (bytes32) {
izeSealed.sol:217:    function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 SizeSealed.sol:474:    function getTimings(uint256 auctionId) external view returns (Timings memory timings) {
SizeSealed.sol:478:    function getAuctionData(uint256 auctionId) external view returns (AuctionData memory data) {
```

## 4. Use preincrement to save gas in loops
**Summary:** `++i` costs less gas than `i++`, especially when it is used in for-loops (--i/i-- too) Saves 5 gas PER LOOP
```solidity
SizeSealed.sol:244:        for (uint256 i; i < bidIndices.length; i++) {
SizeSealed.sol:302:        for (uint256 i; i < seenBidMap.length - 1; i++) {
```


## 5. Use `variable == false|0` -> `!variable` or `variable ==  true` -> `variable`
```solidity
util/ECCMath.sol:27:        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);
SizeSealed.sol:144:        if (quoteAmount == 0 || quoteAmount == type(uint128).max || quoteAmount < a.params.minimumBidQuote) {
SizeSealed.sol:223:        if (sellerPriv == 0) {
SizeSealed.sol:366:        if (baseAmount == 0) {

```


## 6.` >=` COSTS LESS GAS THAN `>`
**Summary:** The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas https://gist.github.com/IllIllI000/3dc79d25acccfa16dee4e83ffdc6ffde 
```solidity
util/CommonTokenMath.sol:54:        if (currentTime > vestingEnd) {
SizeSealed.sol:37:        } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
SizeSealed.sol:66:        if (timings.endTimestamp > timings.vestingStartTimestamp) {
SizeSealed.sol:69:        if (timings.vestingStartTimestamp > timings.vestingEndTimestamp) {
SizeSealed.sol:72:        if (timings.cliffPercent > 1e18) {
SizeSealed.sol:79:            ) > auctionParams.reserveQuotePerBase
SizeSealed.sol:273:                    if (data.previousIndex > bidIndex) revert InvalidSorting();
SizeSealed.sol:289:            if (data.filledBase + baseAmount > data.totalBaseAmount) {
SizeSealed.sol:313:        if (data.filledBase > data.totalBaseAmount) {

```

## 7. Use `abi.encodePacked` for gas optimization 
**Summary:**  Changing the abi.encode function to abi.encodePacked  can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, abi.encodePacked is more gas-efficient.
```solidity
util/ECCMath.sol:26:        bytes memory data = abi.encode(point, scalar);
util/ECCMath.sol:61:        return keccak256(abi.encode(point));
SizeSealed.sol:467:        return keccak256(abi.encode(message));
test/mocks/MockBuyer.sol:34:        salt = bytes16(keccak256(abi.encode("randomsalt")));
test/mocks/MockSeller.sol:97:        auctionContract.reveal(auctionId, SELLER_PRIVATE_KEY, abi.encode(bidIndices, clearingBase, clearingQuote));

```



## 8. Use custom errors to save deployment and runtime costs in case of revert
**Summary:** Instead of using strings for error messages (e.g., `require(msg.sender == owner, “unauthorized”)`), you can use custom errors to reduce both deployment and runtime gas costs. In addition, they are very convenient as you can easily pass dynamic information to them.
```solidity
test/mocks/MockBuyer.sol:44:        require(quoteToken.balanceOf(address(this)) >= quoteAmount);
test/mocks/MockBuyer.sol:81:        require(quoteToken.balanceOf(address(this)) >= quoteAmount);
```

