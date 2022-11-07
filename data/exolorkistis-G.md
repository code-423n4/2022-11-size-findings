[1] ``ABI.ENCODE()`` IS LESS EFFICIENT THAN ``ABI.ENCODEPACKED()``

*There are 3 instances of this issue:*

```
File: 2022-11-size/src/util/ECCMath.sol

26: bytes memory data = abi.encode(point, scalar);

61:  return keccak256(abi.encode(point));

```
[https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L26](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L26)

[https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L61](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L61)

```
File:  2022-11-size/src/SizeSealed.sol

467:   return keccak256(abi.encode(message));

```


[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L467](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L467)


[2]  USAGE OF ``UINTS/INTS`` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

*There are 27 instances of this issue*

```
File: 2022-11-size/src/util/CommonTokenMath.sol

48 :  uint32 vestingStart

49:   uint32 vestingEnd,

50:  uint32 currentTime,

51:   uint128 cliffPercent,

52:  uint128 baseAmount
```
[https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L48-L52](https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L48-L52)

```
File:  2022-11-size/src/SizeSealed.sol


33:  } else if (a.data.lowestQuote != type(uint128).max) {

78:  auctionParams.minimumBidQuote, type(uint128).max, auctionParams.totalBaseAmount

90:   a.data.lowestQuote = type(uint128).max;

124:  uint128 quoteAmount,

144:  if (quoteAmount == 0 || quoteAmount == type(uint128).max || quoteAmount < a.params.minimumBidQuote) {

196:    (uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote) =

197:    abi.decode(finalizeData, (uint256[], uint128, uint128));

205:   uint128 totalBaseAmount;

206:   uint128 filledBase;

217:   function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)

266:   uint128 baseAmount = uint128(uint256(decryptedMessage >> 128));

269:   uint256 quotePerBase = FixedPointMathLib.mulDivDown(b.quoteAmount, type(uint128).max, baseAmount);

319:   uint128 unsoldBase = data.totalBaseAmount - data.filledBase;

365:   uint128 baseAmount = b.filledBaseAmount;

370:   uint128 baseTokensAvailable = tokensAvailableForWithdrawal(auctionId, baseAmount);

398:   if (a.data.lowestQuote != type(uint128).max) {

426:   if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {

451:  function tokensAvailableForWithdrawal(uint256 auctionId, uint128 baseAmount)

454:    returns (uint128 tokensAvailable)

470:    function computeMessage(uint128 baseAmount, bytes16 salt) external pure returns (bytes32) {

405:    a.timings.endTimestamp = type(uint32).max;

460:    uint32(block.timestamp),
```

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L33](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L33)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L78](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L78)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L90](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L90)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L124](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L124)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L144](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L144)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L196-L197](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L196-L197)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L205-L206](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L205-L206)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L266](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L266)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L269](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L269)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L319](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L319)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L365](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L365)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L370](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L370)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L398](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L398)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L426](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L426)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L451](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L451)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L454](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L454)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L470](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L470)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L405](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L405)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L460](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L460)

[3] ``<ARRAY>.LENGTH`` SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A`` FOR``-LOOP

The overheads outlined below are *PER LOOP*, excluding the first loop

-storage arrays incur a Gwarmaccess (100 gas)
-memory arrays use MLOAD (3 gas)
-calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset.

*There are 2 instances of this issue:*

```
File: 2022-11-size/src/SizeSealed.sol

244: for (uint256 i; i < bidIndices.length; i++) {

302:  for (uint256 i; i < seenBidMap.length - 1; i++) {

```

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302)

[4] ++I COSTS LESS GAS THAN I++, ESPECIALLY WHEN IT’S USED IN FOR-LOOPS (--I/I-- TOO)

Saves 5 gas per loop

*There are 2 instances of this issue:*

```
File: 2022-11-size/src/SizeSealed.sol

244: for (uint256 i; i < bidIndices.length; i++) {

302:  for (uint256 i; i < seenBidMap.length - 1; i++) {

```

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302)

[5] ++I/I++ SHOULD BE UNCHECKED{++I}/UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS
 
The ``unchecked`` keyword is new in solidity 0.8.0,so this only applies to that version or higher, which these instances are. This saves 30-40 gas *[PER LOOP](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)*

*There are 2 instances of this issue:*

```
File: 2022-11-size/src/SizeSealed.sol

244: for (uint256 i; i < bidIndices.length; i++) {

302:  for (uint256 i; i < seenBidMap.length - 1; i++) {

```

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244)

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302)

