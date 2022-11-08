## [G-01] Sort the struct
## code snipped
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L203
## recommendation
change the struct can save more gas. change it from

        struct FinalizeData {
        uint256 reserveQuotePerBase;
        uint128 totalBaseAmount;
        uint128 filledBase;
        uint256 previousQuotePerBase;
        uint256 previousIndex;
to

        struct FinalizeData {
        uint128 totalBaseAmount;
        uint128 filledBase;
        uint256 reserveQuotePerBase;
        uint256 previousQuotePerBase;
        uint256 previousIndex;

## [G-02] Cache bidIndices.length
## code snipped
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L227
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L241
## recommendation
caching the array length can reduce gas it caused access to a local variable is more cheap than query storage / calldata / memory in solidity. we recommend to cache the  bidIndices.length. because caching a state variable  replace each Gwarmaccess (100 gas) with cheaper stack read.

## [G-03] x -= y (x += y) costs more gas than x = x – y (x = x + y) for state variables
x -= y costs more gas than x = x – y for state variables.
## code snipped
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L373
## recommendation
split x -= y (x += y) to x = x – y (x = x + y) can saves 113 gas.