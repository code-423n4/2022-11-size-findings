# GAS
## Variables should be cached when used several times
### Summary
Variables read more than once improves gas usage when cached into local variable
### Github Permalinks
b.quoteAmount
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L376-L378

### Mitigation
Cache variables used more than one into a local variable.


## `<X> += <Y>` costs more gas than `<X> = <X> + <Y>` for state variables
### Summary
`x+=y` costs more gas than x=x+y for state variables

### Github Permalinks
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L373
        b.baseWithdrawn += baseTokensAvailable;
### Mitigation
Don't use `+=` for state variables as it cost more gas.


## Unused named returns
### Summary
Using both named returns and a return statement isn’t necessary. Removing one of those can improve code clarity 
### Details
Also as returns variable is ignored, it wastes extra gas

### Github Permalinks
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L454-L463
        returns (uint128 tokensAvailable)


### Mitigation
Remove return or returns when both used




## `abi.encode()` is less gas efficient than `abi.encodePacked()`
### Summary
In general, `abi.encodePacked` is more gas-efficient.

### Details
Changing the abi.encode function to `abi.encodePacked` can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, `abi.encodePacked` is more gas-efficient.
### Github Permalinks
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L467
https://github.com/code-423n4/2022-11-size/blob/59ab86cffde7ba1d2565e97dd7731412a6394183/src/util/ECCMath.sol#L26
https://github.com/code-423n4/2022-11-size/blob/59ab86cffde7ba1d2565e97dd7731412a6394183/src/util/ECCMath.sol#L61
### Mitigation
Consider changing abi.encode to `abi.encodePacked`

[:art:]
## Increments can be unchecked in loops
### Summary
Unchecked operations as the ++i on for loops are cheaper than checked one.

### Details
In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline..

    The code would go from:
```
    for (uint256 i; i < numIterations; i++) {
    // ...
    }
```

    to
```
    for (uint256 i; i < numIterations;) {
      // ...
      unchecked { ++i; }
    }
    The risk of overflow is inexistent for a uint256 here.
```

### Github Permalinks
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L244
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L302

### Mitigation
Add unchecked `++i` at the end of all the for loop where it's not expected to overflow and remove them from the for header

## Internal functions only called once can be inlined to save gas
### Summary
Not inlining costs `20` to `40` gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.
### Github Permalinks
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/util/CommonTokenMath.sol#L53
```
    function tokensAvailableAtTime(
        uint32 vestingStart,
        uint32 vestingEnd,
        uint32 currentTime,
        uint128 cliffPercent,
        uint128 baseAmount
    ) internal pure returns (uint128) {
```

### Mitigation
Consider changing internal function only called once to inline code for gas savings
