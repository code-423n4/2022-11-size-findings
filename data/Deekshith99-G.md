[G-1]  The increment(i++ or ++i)  in for loop post condition can be made unchecked

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. 
This saves 30-40 gas per loop.

i found 2 instances of it
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L244
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L302

and here is the ref - https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked

