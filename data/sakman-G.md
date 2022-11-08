## 1. Place `i++` in an `unchecked` blocks in for-loops

_src/SizeSealed.sol:_ [L244](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol#L244)
[L302](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol#L302)

## 2. Calldata is cheaper than memory for function input

_src/SizeSealed.sol:_ [L217](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol#L217)

_src/util/ECCMath.sol:_ [L25](https://github.com/code-423n4/2022-11-size/tree/main/src/util/ECCMath.sol#L25)
[L37](https://github.com/code-423n4/2022-11-size/tree/main/src/util/ECCMath.sol#L37)
[L60](https://github.com/code-423n4/2022-11-size/tree/main/src/util/ECCMath.sol#L60)

## 3. Use `x < y + 1` in stead of `x <= y`

_src/SizeSealed.sol:_ [L60](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol#L60)
[L63](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol#L63)
[L270](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol#L270)
[L425](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol#L425)
[L426](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol#L426)

_src/util/CommonTokenMath.sol:_ [L56](https://github.com/code-423n4/2022-11-size/tree/main/src/util/CommonTokenMath.sol#L56)
