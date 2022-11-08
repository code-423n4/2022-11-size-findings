1- It's better to catch the length of the for loop before the loop starts:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302

2- Pre-increment cost low gas fees compared to post-increment (++i instead of i++):

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302