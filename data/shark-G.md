## `x += y` is less efficient than `x = x + y`

This is the same for subtracting.

Here is an example:
File: `SizeSealed.sol` [Line 294](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L294)

```
data.filledBase += baseAmount;
```

The above can be changed to:

```
data.filledBase = data.filledBase + baseAmount;
```

There is one more instance of this found:

File: `SizeSealed.sol` [Line 373](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L373)

## Unchecking overflow/underflow saves gas.

Consider for example:

File: `SizeSealed.sol` [Line 302-306](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302-L306)

```
for (uint256 i; i < seenBidMap.length - 1; i++) {
    if (seenBidMap[i] != type(uint256).max) {
        revert InvalidState();
    }
}
```

Each time `i++` is called, underflow/overflow checks are made. However, `i` is already constrained by length, `i < seenBidMap.length - 1`, Thus making those underflow/overflow checks unneeded.

The loop can be rewritten as:

```
for (uint256 i; i < seenBidMap.length - 1; ) {
  if (seenBidMap[i] != type(uint256).max) {
      revert InvalidState();
  }
  unchecked {
      i++;
  }
}
```

There is one more instance of this found:

File: `SizeSealed.sol` [Line 244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244)

## Function Order Affects Gas Consumption

The order of the functions will have an impact on gas consumption. The reason why is because the order is dependent on the Method ID. Each function position will use up an extra 22 gas.

For more info: https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92
