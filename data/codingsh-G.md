 ## G002: Cache Array Length Outside of Loop
```solidity
  2022-11-size/src/SizeSealed.sol::155 => uint256 bidIndex = a.bids.length;
  2022-11-size/src/SizeSealed.sol::195 => if (finalizeData.length != 0) {
  2022-11-size/src/SizeSealed.sol::227 => if (bidIndices.length != a.bids.length) {
  2022-11-size/src/SizeSealed.sol::241 => uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);
  2022-11-size/src/SizeSealed.sol::309 => if (seenBidMap[seenBidMap.length - 1] != (1 << (bidIndices.length % 256)) - 1) {
```
## G006: Use immutable for OpenZeppelin AccessControl's Roles Declarations
```solidity
  2022-11-size/src/SizeSealed.sol::467 => return keccak256(abi.encode(message));
```
 ## G007: Long Revert Strings
```solidity
  2022-11-size/src/SizeSealed.sol::6 => import {SafeTransferLib} from "solmate/utils/SafeTransferLib.sol";
  2022-11-size/src/SizeSealed.sol::7 => import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
```
 ## G008: Use Shift Right/Left instead of Division/Multiplication if possible
```solidity
  2022-11-size/src/SizeSealed.sol::241 => uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);
  2022-11-size/src/SizeSealed.sol::249 => uint256 bitmapIndex = bidIndex / 256;
```