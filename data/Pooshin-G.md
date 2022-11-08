# Gas Optimizations

## [GAS-1] `++i`/`i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as is the case when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop.

There are 2 instances of this issue:

[SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244)
[SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302)

