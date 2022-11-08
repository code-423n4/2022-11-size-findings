## 1) `unchecked` keyword should be used on situations where there is no possibility of overflow/underflow to save gas

[https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L65](https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L65)

## 2) Use `calldata` instead of `memory` on external function to save gas

[https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L37](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L37)
[https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L51](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L51)
[https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L60](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L60)
[https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L25](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L25)

## 3) Avoid redundant return statements to save gas

[https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L56](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L56)
[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L454](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L454)

## 4) Cache storage variable, used consecutively to save gas

Here `a.params.merkleRoot` can be cached
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L132
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L134 

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L29-L37 : Here `a.timings` can be cached to save the key caclulation gas cost

## 5) Use `break` instead of revert inside loop statements to save gas

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L304](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L304)
[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L275](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L275)
[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L298](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L298)
[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L310](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L310)
[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L314](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L314)