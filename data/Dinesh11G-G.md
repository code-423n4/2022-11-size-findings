### Title
Cache Array Length Outside of Loop

#### Impact
Issue Information: Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

Example
🤦 Bad:

for (uint256 i = 0; i < array.length; i++) {
    // invariant: array's length is not changed
}
🚀 Good:

uint256 len = array.length
for (uint256 i = 0; i < len; i++) {
    // invariant: array's length is not changed
}

#### Findings:
```
2022-11-size/src/SizeSealed.sol::155 => uint256 bidIndex = a.bids.length;
2022-11-size/src/SizeSealed.sol::195 => if (finalizeData.length != 0) {
2022-11-size/src/SizeSealed.sol::227 => if (bidIndices.length != a.bids.length) {
2022-11-size/src/SizeSealed.sol::241 => uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);
2022-11-size/src/SizeSealed.sol::244 => for (uint256 i; i < bidIndices.length; i++) {
2022-11-size/src/SizeSealed.sol::302 => for (uint256 i; i < seenBidMap.length - 1; i++) {
2022-11-size/src/SizeSealed.sol::309 => if (seenBidMap[seenBidMap.length - 1] != (1 << (bidIndices.length % 256)) - 1) {
```
#### Tools used
Manual


=================================================================================================



### Title
Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
Issue Information: When dealing with unsigned integer types, comparisons with != 0 are cheaper than with > 0.

Example
🤦 Bad:

// `a` being of type unsigned integer
require(a > 0, "!a > 0");
🚀 Good:

// `a` being of type unsigned integer
require(a != 0, "!a > 0");

#### Findings:
```
2022-11-size/src/test/SizeSealedFuzz.t.sol::70 => endTimeOffset > 0 && b1QuoteAmountBid > 0 && b1BaseAmountBid > 0 && b1QuoteAmountBid != type(uint128).max
2022-11-size/src/test/SizeSealedFuzz.t.sol::164 => vm.assume(endTimeOffset > 0 && vestingEndOffset % 4 == 0 && vestingEndOffset > 0);
```
#### Tools used
Manual


=================================================================================================


### Title
Use immutable for OpenZeppelin AccessControl's Roles Declarations

#### Impact
Issue Information: ⚡️ Only valid for solidity versions <0.6.12 ⚡️

Access roles marked as constant results in computing the keccak256 operation each time the variable is used because assigned operations for constant variables are re-evaluated every time.

Changing the variables to immutable results in computing the hash only once on deployment, leading to gas savings.

Example
🤦 Bad:

bytes32 public constant GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");
🚀 Good:

bytes32 public immutable GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");

#### Findings:
```
2022-11-size/src/SizeSealed.sol::133 => bytes32 leaf = keccak256(abi.encodePacked(msg.sender));
2022-11-size/src/SizeSealed.sol::467 => return keccak256(abi.encode(message));
2022-11-size/src/test/SizeSealed.t.sol::584 => data[0] = keccak256(abi.encodePacked(address(bidder1)));
2022-11-size/src/test/SizeSealed.t.sol::585 => data[1] = keccak256(abi.encodePacked(address(bidder2)));
2022-11-size/src/test/mocks/MockBuyer.sol::25 => uint256 constant SELLER_PRIVATE_KEY = uint256(keccak256("Size Seller"));
2022-11-size/src/test/mocks/MockBuyer.sol::26 => uint256 constant BUYER_PRIVATE_KEY = uint256(keccak256("Size Buyer"));
2022-11-size/src/test/mocks/MockBuyer.sol::34 => salt = bytes16(keccak256(abi.encode("randomsalt")));
2022-11-size/src/test/mocks/MockSeller.sol::20 => uint256 constant SELLER_PRIVATE_KEY = uint256(keccak256("Size Seller"));
2022-11-size/src/util/ECCMath.sol::61 => return keccak256(abi.encode(point));
```
#### Tools used
Manual



=================================================================================================


### Title
Long Revert Strings

#### Impact
Issue Information: Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

Example
🤦 Bad:

require(condition, "UniswapV3: The reentrancy guard. A transaction cannot re-enter the pool mid-swap");
🚀 Good (with shorter string):

// TODO: Provide link to a reference of error codes
require(condition, "LOK");
🚀 Good (with custom errors):

/// @notice A transaction cannot re-enter the pool mid-swap.
error NoReentrancy();

// ...

if (!condition) {
    revert NoReentrancy();
}

#### Findings:
```
2022-11-size/src/SizeSealed.sol::6 => import {SafeTransferLib} from "solmate/utils/SafeTransferLib.sol";
2022-11-size/src/SizeSealed.sol::7 => import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
2022-11-size/src/test/SizeSealed.t.sol::6 => import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
2022-11-size/src/test/SizeSealed.t.sol::108 => assertEq(beforeBase, afterBase, "base before cancel != base after cancel");
2022-11-size/src/test/SizeSealed.t.sol::109 => assertEq(beforeQuote, afterQuote, "quote before cancel != quote after cancel");
2022-11-size/src/test/SizeSealedFuzz.t.sol::5 => import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
2022-11-size/src/test/SizeSealedFuzz.t.sol::101 => assertEq(b1QuoteBeforeBid, b1QuoteAfterBid + b1QuoteAmountBid, "bidder1 quote balance before/after bidding");
2022-11-size/src/test/SizeSealedFuzz.t.sol::109 => assertEq(data.lowestBase, b1BaseAmountBid, "data.lowestBase != b1BaseAmountBid");
2022-11-size/src/test/SizeSealedFuzz.t.sol::110 => assertEq(data.lowestQuote, b1QuoteAmountBid, "data.lowestBase != b1BaseAmountBid");
2022-11-size/src/test/SizeSealedFuzz.t.sol::122 => "Seller Received incorrect quote amount from finalize()"
2022-11-size/src/test/SizeSealedFuzz.t.sol::128 => "Seller Received incorrect base refund from finalize()"
2022-11-size/src/test/SizeSealedFuzz.t.sol::146 => "bidder1 Received incorrect base amount from finalize()"
2022-11-size/src/test/SizeSealedFuzz.t.sol::154 => "bidder1 Received incorrect quote refund from withdraw()"
2022-11-size/src/test/SizeSealedFuzz.t.sol::172 => assertEq(baseBefore, baseAfter + expectedBase, "bidder should not get tokens at t=0");
2022-11-size/src/util/CommonTokenMath.sol::4 => import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
```
#### Tools used
Manual



=================================================================================================


### Title
Use Shift Right/Left instead of Division/Multiplication if possible

#### Impact
Issue Information: A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

Example
🤦 Bad:

uint256 b = a / 2;
uint256 c = a / 4;
uint256 d = a * 8;
🚀 Good:

uint256 b = a >> 1;
uint256 c = a >> 2;
uint256 d = a << 3;

#### Findings:
```
2022-11-size/src/SizeSealed.sol::241 => uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);
2022-11-size/src/SizeSealed.sol::249 => uint256 bitmapIndex = bidIndex / 256;
2022-11-size/src/test/SizeSealed.t.sol::530 => vm.warp((unlockTime + unlockEnd) / 2);
2022-11-size/src/test/SizeSealed.t.sol::553 => vm.warp((unlockTime + unlockEnd) / 2);
2022-11-size/src/test/SizeSealedFuzz.t.sol::131 => vm.warp(uint32(_startTime) + uint32(endTimeOffset) + uint32(vestingStartOffset) + uint32(vestingEndOffset / 2));
2022-11-size/src/test/SizeSealedFuzz.t.sol::174 => vm.warp(vestingStart + uint32(vestingEndOffset) / 4);
2022-11-size/src/test/SizeSealedFuzz.t.sol::175 => expectedBase = bidderBaseDefault / 4;
2022-11-size/src/test/SizeSealedFuzz.t.sol::179 => vm.warp(vestingStart + uint32(vestingEndOffset) / 2);
2022-11-size/src/test/SizeSealedFuzz.t.sol::180 => expectedBase = bidderBaseDefault / 2;
2022-11-size/src/test/SizeSealedFuzz.t.sol::184 => vm.warp(vestingStart + uint32(vestingEndOffset) * 3 / 4);
2022-11-size/src/test/SizeSealedFuzz.t.sol::185 => expectedBase = bidderBaseDefault * 3 / 4;
```
#### Tools used
Manual