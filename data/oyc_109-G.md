## [G-01] Cache Array Length Outside of Loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

```
2022-11-size/src/SizeSealed.sol::244 => for (uint256 i; i < bidIndices.length; i++) {
2022-11-size/src/SizeSealed.sol::302 => for (uint256 i; i < seenBidMap.length - 1; i++) {
```

## [G-02] Use Shift Right/Left instead of Division/Multiplication if possible

A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

```
2022-11-size/src/SizeSealed.sol::241 => uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);
2022-11-size/src/SizeSealed.sol::249 => uint256 bitmapIndex = bidIndex / 256;
```

## [G-03] Use calldata instead of memory

Use calldata instead of memory for function parameters saves gas if the function argument is only read.

```
2022-11-size/src/util/ECCMath.sol::25 => function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {
2022-11-size/src/util/ECCMath.sol::60 => function hashPoint(Point memory point) internal pure returns (bytes32) {
```

## [G-04] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```
2022-11-size/src/SizeSealed.sol::205 => uint128 totalBaseAmount;
2022-11-size/src/SizeSealed.sol::206 => uint128 filledBase;
2022-11-size/src/SizeSealed.sol::365 => uint128 baseAmount = b.filledBaseAmount;
2022-11-size/src/SizeSealed.sol::370 => uint128 baseTokensAvailable = tokensAvailableForWithdrawal(auctionId, baseAmount);
2022-11-size/src/interfaces/ISizeSealed.sol::42 => uint128 quoteAmount;
2022-11-size/src/interfaces/ISizeSealed.sol::43 => uint128 filledBaseAmount;
2022-11-size/src/interfaces/ISizeSealed.sol::44 => uint128 baseWithdrawn;
2022-11-size/src/interfaces/ISizeSealed.sol::56 => uint32 startTimestamp;
2022-11-size/src/interfaces/ISizeSealed.sol::57 => uint32 endTimestamp;
2022-11-size/src/interfaces/ISizeSealed.sol::58 => uint32 vestingStartTimestamp;
2022-11-size/src/interfaces/ISizeSealed.sol::59 => uint32 vestingEndTimestamp;
2022-11-size/src/interfaces/ISizeSealed.sol::60 => uint128 cliffPercent;
2022-11-size/src/interfaces/ISizeSealed.sol::65 => uint128 lowestBase;
2022-11-size/src/interfaces/ISizeSealed.sol::66 => uint128 lowestQuote;
2022-11-size/src/interfaces/ISizeSealed.sol::80 => uint128 totalBaseAmount;
2022-11-size/src/interfaces/ISizeSealed.sol::81 => uint128 minimumBidQuote;
```

## [G-05] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, for example when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop

```
2022-11-size/src/SizeSealed.sol::84 => uint256 auctionId = ++currentAuctionId;
2022-11-size/src/SizeSealed.sol::244 => for (uint256 i; i < bidIndices.length; i++) {
2022-11-size/src/SizeSealed.sol::302 => for (uint256 i; i < seenBidMap.length - 1; i++) {
```

## [G-06] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

use <x> = <x> + <y> or <x> = <x> - <y> instead to save gas

```
2022-11-size/src/SizeSealed.sol::294 => data.filledBase += baseAmount;
2022-11-size/src/SizeSealed.sol::373 => b.baseWithdrawn += baseTokensAvailable;
```

## [G-07] abi.encode() is less efficient than abi.encodePacked()

use abi.encodePacked() where possible to save gas

```
2022-11-size/src/SizeSealed.sol::467 => return keccak256(abi.encode(message));
2022-11-size/src/util/ECCMath.sol::26 => bytes memory data = abi.encode(point, scalar);
2022-11-size/src/util/ECCMath.sol::61 => return keccak256(abi.encode(point));
```

## [G-08] Prefix increments cheaper than Postfix increments

++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)
Saves 5 gas PER LOOP

```
2022-11-size/src/SizeSealed.sol::244 => for (uint256 i; i < bidIndices.length; i++) {
2022-11-size/src/SizeSealed.sol::302 => for (uint256 i; i < seenBidMap.length - 1; i++) {
```

## [G-09] Not using the named return variables when a function returns, wastes deployment gas

It is not necessary to have both a named return and a return statement.

```
2022-11-size/src/util/ECCMath.sol::54 => returns (bytes32 decryptedMessage)
```

## [G-10] Using storage instead of memory for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read.

Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

```
2022-11-size/src/SizeSealed.sol::241 => uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);
```

## [G-11] internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
2022-11-size/src/util/ECCMath.sol::18 => function publicKey(uint256 privateKey) internal view returns (Point memory) {
```