Issue 1:
Event is missing indexed field:

Placing the “indexed” keyword in front of a parameter name will store it as a topic in the log record and make the field more quickly accessible to off-chain tools that parse events. Without the keyword “indexed”, it will be stored as data.

There are 8 instances of this issue:

https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L97
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L101
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L103-L112
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L114
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L116
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L118
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L118
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L118

Issue 2: 
Code is missing Natspec:

https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L28-L34
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L40-L48
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L63-L68
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L86-L91

