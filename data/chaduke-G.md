G1: https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L144-L146
Since ``quoteAmount < a.params.minimumBidQuote`` already covers the case ``quoteAmount == 0``, so we can drop condition 1 to save gas
```
if (quoteAmount == type(uint128).max || (quoteAmount < a.params.minimumBidQuote)) {
            revert InvalidBidAmount();
}
```

G2: https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L223
The privateKey == 0 check should be performed at the beginning of the ``reveal`` function to save gas.


G3: https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L241
use shifting to save gas
```
uint256[] memory seenBidMap = new uint256[]((bidIndices.length >> 8 )+1);

```
G4. https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L270-L277
InvalidSorting should be checked with a separate function and then be called at the beginning of ``finalize()``. 

G5. https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L217
Dupliate detection in argument ``bidIndices`` should be accomplished by another inner function, which should be called at the beginning of ``finalize()``.

G6. https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L313-L315
This check is not necessary since in the for loop (L289-291), we ensure that we will never exceed the ``data.totalBaseAmount``.

G7. https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L28-L43
for the modifier ``atState``, we can cache ``a.times.startTimestamp`` and ``a.times.endTimestamp`` to save gas as they are accessed multiple times

G8.  https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L376
Cache ``b.quoteAmount`` as it is accessed twice.

G9. https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L425
Cache ``a.timing.endTimestamp`` as it is accessed twice
