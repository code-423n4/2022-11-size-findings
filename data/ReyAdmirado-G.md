
# Gas

| | issue |
| ----------- | ----------- |
| 1 | [state variables should be cached in stack variables rather than re-reading them from storage](#1-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage) |
| 2 | [Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `require()` or `if` statement](#2-add-unchecked--for-subtractions-where-the-operands-cannot-underflow-because-of-a-previous-require-or-if-statement) |
| 3 | [`<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables](#3-x--y-costs-more-gas-than-x--x--y-for-state-variables) |
| 4 | [not using the named return variables when a function returns, wastes deployment gas](#4-not-using-the-named-return-variables-when-a-function-returns-wastes-deployment-gas) |
| 5 | [can make the variable outside the loop to save gas](#5-can-make-the-variable-outside-the-loop-to-save-gas) |
| 6 | [`++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as is the case when used in for-loop and while-loops](#6-ii-should-be-uncheckediuncheckedi-when-it-is-not-possible-for-them-to-overflow-as-is-the-case-when-used-in-for-loop-and-while-loops) |
| 7 | [internal functions only called once can be inlined to save gas](#7-internal-functions-only-called-once-can-be-inlined-to-save-gas) |
| 8 | [abi.encode() is less efficient than abi.encodepacked()](#8-abiencode-is-less-efficient-than-abiencodepacked) |


## 1. state variables should be cached in stack variables rather than re-reading them from storage

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses. 

because the `if (a.params.merkleRoot != bytes32(0))` check will usually pass should cache `a.params.merkleRoot` before if statement to save gas
- [SizeSealed.sol#L132](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L132)


## 2. Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `require()` or `if` statement

require(a <= b); x = b - a => require(a <= b); unchecked { x = b - a }
if(a <= b); x = b - a => if(a <= b); unchecked { x = b - a }
this will stop the check for overflow and underflow so it will save gas

- [SizeSealed.sol#L290](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L290)
- [SizeSealed.sol#L319](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L319)


## 3. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
Using the addition operator instead of plus-equals saves gas

- [SizeSealed.sol#L373](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L373)


## 4. not using the named return variables when a function returns, wastes deployment gas

- [SizeSealed.sol#L454](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L454)


## 5. can make the variable outside the loop to save gas

- [SizeSealed.sol#L245](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L245)
- [SizeSealed.sol#L249](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L249)
- [SizeSealed.sol#L250](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L250)
- [SizeSealed.sol#L251](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L251)


## 6. `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as is the case when used in for-loop and while-loops

In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

- [SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244)
- [SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302)


## 7. internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

`decryptMessage`
- [ECCMath.sol#L51](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L51)

`tokensAvailableAtTime`
- [CommonTokenMath.sol#L47](https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L47)


## 8. abi.encode() is less efficient than abi.encodepacked()

- [ECCMath.sol#L26](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L26)
