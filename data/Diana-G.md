## G-01 x += y COSTS MORE GAS THAN x = x + y FOR STATE VARIABLES (SAME APPLIES FOR x -= y , x = x - y)

_There are 2 instances of this issue_

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

```
File: src/SizeSealed.sol

294: data.filledBase += baseAmount;
373: b.baseWithdrawn += baseTokensAvailable;
```

-----------

## G-02 USAGE OF UINTS OR INTS SMALLER THAN 32 BYTES (26 BITS) INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

_There are 18 instances of this issue_

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

```
File: src/SizeSealed.sol

124: uint128 quoteAmount,
196: (uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote) =
197: abi.decode(finalizeData, (uint256[], uint128, uint128));
217: function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
266: uint128 baseAmount = uint128(uint256(decryptedMessage >> 128));
319: uint128 unsoldBase = data.totalBaseAmount - data.filledBase;
365: uint128 baseAmount = b.filledBaseAmount;
370: uint128 baseTokensAvailable = tokensAvailableForWithdrawal(auctionId, baseAmount);
451: function tokensAvailableForWithdrawal(uint256 auctionId, uint128 baseAmount)
454: returns (uint128 tokensAvailable)
470: function computeMessage(uint128 baseAmount, bytes16 salt) external pure returns (bytes32) {
```

https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol

```
File: src/interfaces/ISizeSealed.sol

107: uint128 quoteAmount,
```

https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol

```
File: src/util/CommonTokenMath.sol

48: uint32 vestingStart,
49: uint32 vestingEnd,
50: uint32 currentTime,
51: uint128 cliffPercent,
52: uint128 baseAmount
53: ) internal pure returns (uint128) {
```

----------

## G-03 USING BOOLS FOR STORAGE INCURS OVERHEAD

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol

```
File: src/util/ECCMath.sol

28: (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(data);
```

-------

## G-04 INCREMENTS CAN BE UNCHECKED

In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline

Prior to Solidity 0.8.0, arithmetic operations would always wrap in case of under- or overflow leading to widespread use of libraries that introduce additional checks.

Since Solidity 0.8.0, all arithmetic operations revert on over- and underflow by default, thus making the use of these libraries unnecessary.

To obtain the previous behaviour, an unchecked block can be used

_There are 2 instances of this issue_

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

```
File: src/SizeSealed.sol

244: for (uint256 i; i < bidIndices.length; i++) {
302: for (uint256 i; i < seenBidMap.length - 1; i++) {
```

----------

## G-05 INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS

Not inlining costs 20 to 40 gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol

```
File: src/util/CommonTokenMath.sol

47: function tokensAvailableAtTime(
```

----------------

## G-06 INTERNAL FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS

If the functions are required by an interface, the contract should inherit from that interface and use the `override` keyword

_There is 1 instance of this issue:_

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol

```
File: src/util/ECCMath.sol

37: function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)
```

-----------

## G-07 USING BOTH NAMED RETURNS AND A RETURN STATEMENT ISN'T NECESSARY

Removing unused named returns variables can reduce gas usage (MSTOREs/MLOADs) and improve code clarity. To save gas and improve code quality: consider using only one of those.

_There are 2 instances of this issue_

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

```
File: src/SizeSealed.sol

451-454: function tokensAvailableForWithdrawal(uint256 auctionId, uint128 baseAmount)
        public
        view
        returns (uint128 tokensAvailable)
```

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol

```
File: src/util/ECCMath.sol

51-54: function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)
        internal
        pure
        returns (bytes32 decryptedMessage)
```

-----------
