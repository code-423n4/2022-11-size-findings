# Gas Optimizations : Size

## 1. Caching array length in for loops

**Line References and Details**

[SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L244)
[SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L302)

Caching the array length in `for` loops saves gas.

    uint256 bidIndicesLength = bidIndices.length;
    for (uint256 i; i < bidIndicesLength;++i) {/* lines of code */}

    uint256 seenBidMapLength = seenBidMap.length - 1;
    for (uint256 i; i < seenBidMapLength; ++i) {/* lines of code */}

Estimated Gas Saved: 450 gas units.

## 2. Using Bit Manipulation for Division By Exponents of 2

[SizeSealed.sol#L249](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L249)

Using Right Shift by 8(= 2^8) instead of division by 256 saves gas

    uint256 bitmapIndex = bidIndex / 256;

replace with
    
    uint256 bitmapIndex = bidIndex >> 8;

## 3. Using Pre-increment Instead of Post-Increment of Loop Variable

[SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L244)
[SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L302)

    for (uint256 i; i < seenBidMapLength; ++i)
    for (uint256 i; i < bidIndicesLength; ++i)

Using pre-increment instead of post-increment for `i` in the for loop saves 5 gas units per iteration.