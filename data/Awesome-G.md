## Uncheck arithmetics operations that can’t underflow/overflow

Solidity version 0.8+ comes with an implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible, some gas can be saved by using an unchecked block.
For example:

[Line 302-305](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302-L305)

```
        for (uint256 i; i < seenBidMap.length - 1; i++) {
            if (seenBidMap[i] != type(uint256).max) {
                revert InvalidState();
            }
```

## Using `Calldata` Instead of `Memory` for Read-Only Arguments in External Functions Saves Gas

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 \* <mem_array>.length). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having `memory` arguments, it’s still valid for implementation contracts to use `calldata` arguments instead.

If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore memory is used in the `external` call, it’s still more gas-efficient to use `calldata` when the external function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one

Note that I’ve also flagged instances where the function is `public` but can be marked as `external` since it’s not called by the contract, and cases where a constructor is involved

For example:
[Line 217](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217)

```
Line 217:    function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
```

## Function Order Affects Gas Consumption

public/external function names and public member variable names can be optimized to save gas. See [this](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, per sorted position shifted
