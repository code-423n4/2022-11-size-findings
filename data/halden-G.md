## [G-01] Cache storage values in memory to minimize SLOADs
cache `a.timings` in memory
File SizeSealed.sol: [29-37](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L29-L37)
```
        if (block.timestamp < a.timings.startTimestamp) { // 1 SLOAD
            if (_state != States.Created) revert InvalidState();
        } else if (block.timestamp < a.timings.endTimestamp) { // 2 SLOAD
            if (_state != States.AcceptingBids) revert InvalidState();
        } else if (a.data.lowestQuote != type(uint128).max) { 
            if (_state != States.Finalized) revert InvalidState();
        } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) { // 3 SLOAD
            if (_state != States.RevealPeriod) revert InvalidState();
        } else if (block.timestamp > a.timings.endTimestamp + 24 hours) { // 4 SLOAD
```

## [G-02] Use memory instaed of storage 
`idToAuction[auctionId]` can be stored first in memory and after getting/setting of all variables can be stored in storage

File SizeSealed.sol: [86-92](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L86-L92), [131](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L131), [181](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L181), [221](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L221), [246](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L246), [337-338](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L337-L338), [359-360](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L359-L360), [392](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L392), [418-419](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L418-L419), [456](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L456)

## [G-03] Using unchecked blocks to save gas - Increments in for loop can be unchecked
The majority of Solidity for loops increment a uint256 variable that starts at 0. These increment operations never need to be checked for over/underflow because the variable will never reach the max number of uint256 (will run out of gas long before that happens). The default over/underflow check wastes gas in every iteration of virtually every for loop . eg.

File SizeSealed.sol: [244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244), [302](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302)

## [G-04] Add unchecked {} where the operands can not underflow/overflow because of a previous check
`uint128 unsoldBase = data.totalBaseAmount - data.filledBase;`
File SizeSealed.sol: [319](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L319)
is checked in [313](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L319) line 


`currentTime - vestingStart` and `vestingEnd - vestingStart` can be unchecked
File CommonTokenMath.sol: [65](https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L65)

## [G-05] X += Y costs more gas than  X = X + Y for state variables
File SizeSealed.sol: [373](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L373)