## Redundant built-in safe arithmetics

Instances (2) : ``SizeSealed.sol#244, 302``

Avoid unnecessary built-in safe arithmetics. In the following `for` loop, each increment is performed with bound-checks when they are entirely redundant :

SizeSealed.sol#244:
```
for (uint256 i; i < bidIndices.length; i++) {
	(...)
}
```

SizeSealed.sol#302:
```
for (uint256 i; i < seenBidMap.length - 1; i++) {
	(...)
}`
```

Due to the inherent constraints of Solidity, ``bidIndices.length``  and ``seenBidMap.length`` are guaranteed to fit within a ``uint256`` variable, meaning that performing a safe incrementation on each loop for the `i` variable is redundant and costly. 

To optimize such code blocks, it's recommended to relocate the incrementation to the end of the loop, inside an ``unchecked`` code block, saving around 120 gas for each iteration:

```
for (uint256 i; i < bidIndices.length;) {
	(...)
	unchecked {++i;}
}
```
```
for (uint256 i; i < seenBidMap.length;) {
	(...)
	unchecked {++i;}
}
```


