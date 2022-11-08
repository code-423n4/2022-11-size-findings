# USING CALLDATA INSTEAD OF MEMORY FOR READ-ONLY ARGUMENTS IN EXTERNAL FUNCTIONS SAVES GAS

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having memory arguments, it’s still valid for implementation contracs to use calldata arguments instead.

If the array is passed to an internal function which passes the array to another internal function where the array is modified and therefore memory is used in the external call, it’s still more gass-efficient to use calldata when the external function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one.

```solidity
File: SizeSealed.sol
217:     function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
218:         public
219:         atState(idToAuction[auctionId], States.RevealPeriod)
```

# INCREMENTS/DECREMENTS CAN BE UNCHECKED IN FOR-LOOPS

In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

Consider wrapping with an unchecked block here (around 25 gas saved per instance):

sample:
```solidity
File: SizeSealed.sol
302:         for (uint256 i; i < seenBidMap.length - 1; i++) {
303:             if (seenBidMap[i] != type(uint256).max) {
304:                 revert InvalidState();
305:             }
306:         }
```

can be change to:
```solidity
        for (uint256 i; i < seenBidMap.length - 1; ) {
            if (seenBidMap[i] != type(uint256).max) {
                revert InvalidState();
            }
            unchecked { ++i; }
        }
```