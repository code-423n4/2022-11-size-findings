1.
Title: Using `storage` instead of `memory` for struct can save gas

Proof of Concept:
[ECCMath.sol#L42](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L42)

Recommended Mitigation Steps:
Replace `memory` with `storage`
________________________________________________________________________

2.
Title: Caching `length` for loop can save gas

Proof of Concept:
[SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244)

Recommended Mitigation Steps:
Change to:

``` 
	uint256 Length = bidIndices.length;
	for (uint256 i; i < Length; i++) {
```
________________________________________________________________________

3.
Title: Unchecking arithmetics operations that can't underflow/overflow

Proof of Concept:
[SizeSealed.sol#L290](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L290) Should be unchecked due to L#289

Recommended Mitigation Steps:
Use `unchecked`
________________________________________________________________________

4.
Title: Using unchecked and prefix increment is more effective for gas saving:

Proof of Concept:
[SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244)
[SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302)

Recommended Mitigation Steps:
Change to:

```
for (uint256 i; i < bidIndices.length;) {
// ...
unchecked { ++i; }
}
```
________________________________________________________________________