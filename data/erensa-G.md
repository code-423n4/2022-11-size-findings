Report 1

An array's length should be cached to save gas in for-loops 

Detail
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack. catching the array length in the stack saves around 3 gas per iteration.
Here, I suggest storing the array's length in a variable before the for-loop, ad use it instead:

'++i' costs less gas compared to 'i++'
 
 '++1' costs less gas compared to 'i++'  for unsigned integer, as pre-increment is cheaper (about 5 agas per iteration)
 
 'i++' increment 'i and returns the initial value of 'i' . which means:
 
 uint i = 1;
 i++; // == 1 but i  == 2
 
 but '++i' return the actual increment value 
 
 uint i = 1;
 ++i // == 2 and i == 2 too, so need for a temporary variable
 
 
PoC
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L244

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L302


Recommended Mitigation Steps
I suggest using '++i' Instead of 'i++' to increment of the value of an uint variable.



Report 2

usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

Detail 
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

PoC
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L336

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L358

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L391

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L451


Recommended Mitigation Steps
Use a larger size then downcast where needed