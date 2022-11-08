### G-01 X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES

*Number of Instances Identified: 2*

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

```
294: data.filledBase += baseAmount;
373: b.baseWithdrawn += baseTokensAvailable;
```

### G-02 INTERNAL FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS

*Number of Instances Identified: 1*

If the functions are required by an interface, the contract should inherit from that interface and use the `override` keyword

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol

```
37: function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)
```


### G-03 USING BOOLS FOR STORAGE INCURS OVERHEAD

*Number of Instances Identified: 1*

```
    // Booleans are more expensive than uint256 or any type that takes up a full    // word because each write operation emits an extra SLOAD to first read the    // slot's contents, replace the bits taken up by the boolean, and then write    // back. This is the compiler's defense against contract upgrades and    // pointer aliasing, and it cannot be disabled.
```

[https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess for the extra SLOAD, and to avoid Gsset (**20000 gas**) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol

```
28: (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(data);
```

### G-04 ++I or I++ SHOULD BE UNCHECKED{++I} or UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS

*Number of Instances Identified: 2*

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

```
244: for (uint256 i; i < bidIndices.length; i++) {
302: for (uint256 i; i < seenBidMap.length - 1; i++) {
```


### G-05 USING BOTH NAMED RETURNS AND A RETURN STATEMENT ISN'T NECESSARY

*Number of Instances Identified: 2*

Removing unused named returns variables can reduce gas usage (MSTOREs/MLOADs) and improve code clarity. To save gas and improve code quality: consider using only one of those.

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

```
454: returns (uint128 tokensAvailable)
```

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol

```
54: returns (bytes32 decryptedMessage)
```

### G-05 INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS

*Number of Instances Identified: 1*

Not inlining costs **20 to 40 gas** because of two extra `JUMP` instructions and additional stack operations needed for function calls.

https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol

```
47: function tokensAvailableAtTime()
```

### G-06 USAGE OF UINTS or INTS SMALLER THAN 32 BYTES INCURS OVERHEAD

*Number of Instances Identified: 14*

> When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

```
124: uint128 quoteAmount
196: uint128 clearingBase, uint128 clearingQuote
197: abi.decode(finalizeData, (uint256[], uint128, uint128));
217: uint128 clearingBase, uint128 clearingQuote)
266: uint128 baseAmount
319: uint128 unsoldBase
365: uint128 baseAmount
370: uint128 baseTokensAvailable
470: uint128 baseAmount
```

https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol

```
48: uint32 vestingStart
49: uint32 vestingEnd
50: uint32 currentTime
51: uint128 cliffPercent
52: uint128 baseAmount
```