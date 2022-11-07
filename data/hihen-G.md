* Redundant if statement branches can be simplified to save gas
  The if branches in [SizeSealed.sol#L35-L41](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L35-L41)
    ```
    } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
        if (_state != States.RevealPeriod) revert InvalidState();
    } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
        if (_state != States.Voided) revert InvalidState();
    } else {
        revert();
    }
    ```
  could be simplified to:
    ```
    } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
        if (_state != States.RevealPeriod) revert InvalidState();
    } else {
        if (_state != States.Voided) revert InvalidState();
    }
    ```
* Expressions that will never overflow could be unchecked
  - [SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244)
    ```
    for (uint256 i; i < bidIndices.length; i++) {
    ```
  - [SizeSealed.sol#L249-L251](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L249-L251)
    ```
    uint256 bitmapIndex = bidIndex / 256;
    ...
    uint256 indexBit = 1 << (bidIndex % 256);
    ```
  - [SizeSealed.sol#L289-L294](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L289-L294)
    ```
    if (data.filledBase + baseAmount > data.totalBaseAmount) {
        baseAmount = data.totalBaseAmount - data.filledBase;
    }
    ...
    data.filledBase += baseAmount;
    ```
  - [SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L302)
    ```
    for (uint256 i; i < seenBidMap.length - 1; i++) {
    ```
* An array's length should be cached to save gas in for-loops
  - [SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L244)
    ```
    for (uint256 i; i < bidIndices.length; i++) {
    ```
  - [SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L302)
    ```
    for (uint256 i; i < seenBidMap.length - 1; i++) {
    ```
