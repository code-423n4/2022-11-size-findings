## Use `calldata` instead of `memory`

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory.

Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

```
    function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)
```
>File: src/util/ECCMath.sol [(line 37)](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L37)

```
    function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)
```
>File: src/util/ECCMath.sol [(line 51)](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L51)

```
	Other instances of this issue are:
```
* https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L60
* https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L25
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217

## Use `unchecked` to save gas

```
                        baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart

```
>File: src/util/CommonTokenMath.sol [(line 65)](https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L65) 

## Ununsed named `return`

Using both named returns and a return statement isn’t necessary. Removing one of those can improve code clarity:


```
        return encryptedMessage ^ hashPoint(sharedPoint);
```
>File: src/util/ECCMath.sol [(line 56)](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L56) 
```
        return CommonTokenMath.tokensAvailableAtTime(
```
>File: src/SizeSealed.sol [(line 56)](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L457) 

## `internal` functions only called once can be `inlined` to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
    function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)
        internal
```
>File: src/util/ECCMath.sol [(line 37-38)](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L37-L38)

## Cache `a.timings` to save gas
`a.timings`  is used multiple times cache it in a memory variable to save gas.
```
    ///@audit: `a.timings` is used more than one time
        if (block.timestamp < a.timings.startTimestamp) {
            if (_state != States.Created) revert InvalidState();
        } else if (block.timestamp < a.timings.endTimestamp) {
            if (_state != States.AcceptingBids) revert InvalidState();
        } else if (a.data.lowestQuote != type(uint128).max) {
            if (_state != States.Finalized) revert InvalidState();
        } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
            if (_state != States.RevealPeriod) revert InvalidState();
        } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
```
>File: src/SizeSealed.sol [(line 29-37)](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L29-L37)


## `internal` functions not called by the contract should be removed to save deployment gas

```
    function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)
        internal
```
>File: src/util/ECCMath.sol [(line 51-52)](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L51-L52)


## `abi.encode()` is less efficient than `abi.encodePacked()`

```
        return keccak256(abi.encode(message));
```
>File: src/SizeSealed.sol [(line 467)](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L467) 
