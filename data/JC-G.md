

## Summary

- G-01 internal functions only called once can be inlined to save gas
- G-02 Cache array length before loop
- G-03 abi.encode() is less efficient than abi.encodePacked()
- G-04 Caching storage values in memory
- G-05 Use calldata instead of memory for function parameters
- G-06 Use simple comparison in trinary logic / in if statement

Total: 22 instances in 6 issues.


## G-01 internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

Instances (2):

File: ECCMath.sol

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L37
https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L51



## G-02 Cache array length before loop 

Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack. 
Caching the array length in the stack saves around 3 gas per iteration. 
Here, I suggest storing the array’s length in a variable before the for-loop, and use it instead

Instances (2):

File: SizeSealed.sol

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302



## G-03 abi.encode() is less efficient than abi.encodePacked()

Instances (2):

File: SizeSealed.sol

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L467

File: ECCMath.sol

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L26

 
## G-04 Caching storage values in memory

Anytime you are reading from storage more than once, it is cheaper in gas cost to cache the variable in memory: a SLOAD cost 100gas, while MLOAD and MSTORE cost 3 gas. 
In particular, in for loops, when using the length of a storage array as the condition being checked after each loop, caching the array length in memory can yield significant gas savings if the array length is high.

Instances (11):

File: SizeSealed.sol

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L86
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L131
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L181
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L221
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L246
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L337
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L338
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L392
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L418
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L419
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L456


## G-05 Use calldata instead of memory for function parameters

Having function arguments use calldata instead of memory can save gas. 

Recommended Mitigation Steps: 
Change function arguments from memory to calldata.

Instance (1):

File: SizeSealed.sol

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217


## G-06 Use simple comparison in trinary logic / in if statement 

The comparison operators >= and <= use more gas than >, <, or ==. 
Replacing the >= and ≤ operators with a comparison operator that has an opcode in the EVM saves gas. 

Recommended Mitigation Steps: 
Replace the comparison operator and reverse the logic to save gas using the suggestions above.

Instances (4):

File: SizeSealed.sol

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L63
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L157
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L270
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L425





