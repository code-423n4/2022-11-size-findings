## 1) Use `indexed` keyword in events

[https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L114](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L114)
[https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L116](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L116)
[https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L118](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L118)
[https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L120](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L120)
[https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L122](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L122)

## 2) Usage of uint , whose size is less than 256 bits incurs overhead

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L205-L206
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L365
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L451

## 3) Usage of `block.timestamp` is risky

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L31
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L35
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L60
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L425
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L426

## 4) Typos should be avoided

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L112 - running
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L431 -  further

## 5) Use `non-reentrant` modifier to prevent re-entrancy

On functions like `withdraw` and `refund` , `non-reentrant` modifier should be used.
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L358
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L336
