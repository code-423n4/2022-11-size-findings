G1: https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L144-L146
Since ``quoteAmount < a.params.minimumBidQuote`` already covers the case ``quoteAmount == 0``, so we can drop condition 1 to save gas
```
if (quoteAmount == type(uint128).max || (quoteAmount < a.params.minimumBidQuote)) {
            revert InvalidBidAmount();
}
```
G2. https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L280
change *continue* to *break* since all future ``quotePerbase`` will be even smaller or equal, no need to continue. This will save much gas.

G3: https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L223
The privateKey == 0 check should be performed at the beginning of the ``reveal`` function to save gas.


G4: https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L241
use shifting to save gas
``
uint256[] memory seenBidMap = new uint256[]((bidIndices.length >> 8 )+1);

```
G5. https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L270-L277
InvalidSorting should be checked with a separate function and then be called at the beginning of ``finalize()``. 