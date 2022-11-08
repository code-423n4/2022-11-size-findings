# Index
1. I = I + (-) X is cheaper in gas cost than I += (-=) X
2. ABI.ENCODE() is less efficient than ABI.ENCODEPACKED()
3. Use unchecked in for/while loops when it's not possible to overflow
4. Calldata vs Memory
5. Multiplication/division by two should use bit shifting
6. Remove local variables that are used once

# Details
## 1. I = I + (-) X is cheaper in gas cost than I += (-=) X
### Description
In the following example (optimizer = 10000) it's possible to demostrate that I = I + X cost less gas than I += X in state variable.

```solidity
contract Test_Optimization {
    uint256 a = 1;
    function Add () external returns (uint256) {
        a = a + 1;
        return a;
    }
}

contract Test_Without_Optimization {
    uint256 a = 1;
    function Add () external returns (uint256) {
        a += 1;
        return a;
    }
}
```
* Test_Optimization cost is 26324 gas
* Test_Without_Optimization cost is 26440 gas

With this optimization it's possible to **save 116 gas**

### Lines in the code
[SizeSealed.sol#L294](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L294)
[SizeSealed.sol#L373](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L373)
	
## 2. ABI.ENCODE() is less efficient than ABI.ENCODEPACKED()
### Description
**abi.encode** will apply [ABI encoding rules](https://docs.soliditylang.org/en/v0.8.17/abi-spec.html). Therefore all elementary types are padded to 32 bytes and dynamic arrays include their length. 
Therefore it is possible to also decode this data again (with abi.decode) when the type are known.

**abi.encodePacked** will only use the only use the minimal required memory to encode the data. E.g. an address will only use 20 bytes and for dynamic arrays only the elements will be stored without length. 
For more info see the [Solidity docs for packed mode](https://docs.soliditylang.org/en/v0.8.17/abi-spec.html?highlight=encodepacked#non-standard-packed-mode)

For the input of the keccak method it is important that you can ensure that the resulting bytes of the encoding are unique. So if you always encode the same types and arrays 
always have the same length then there is no problem. But if you switch the parameters that you encode or encode multiple dynamic arrays you might have conflicts.

For example:

abi.encodePacked(address(0x0000000000000000000000000000000000000001), uint(0)) encodes to the same as abi.encodePacked(uint(0x0000000000000000000000000000000000000001000000000000000000000000), address(0))

and

abi.encodePacked(uint[](1,2), uint[](3)) encodes to the same as abi.encodePacked(uint[](1), uint[](2,3))

Therefore these examples will also generate the same hashes even so they are different inputs.

On the other hand you require less memory and therefore in most cases abi.encodePacked uses less gas than abi.encode.

### Lines in the code
[SizeSealed.sol#L467](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L467)
[ECCMath.sol#L26](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L26)
[ECCMath.sol#L61](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L61)
	
## 3. Use unchecked in for/while loops when it's not possible to overflow
### Description
Use unchecked { i++; } or unchecked{ ++i; } when it's not possible to overflow to save gas.

### Lines in the code	
[SizeSealed.sol#L244](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L244)
[SizeSealed.sol#L302](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L302)
	
## 4. Calldata vs Memory
### Description
Use calldata instead of memory in a function parameter when you are only to read the data can save gas by storing it in calldata

### Lines in the code

[SizeSealed.sol#L217](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L217)
[ECCMath.sol#L25](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L25)
[ECCMath.sol#L37](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L37)
[ECCMath.sol#L51](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L51)
[ECCMath.sol#L60](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L60)
	
	
## 5. Multiplication/division by two should use bit shifting
### Description
<x> * 2 is equivalent to <x> << 1 and <x> / 2 is the same as <x> >> 1. The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas.
In the code there are some division by 256 that is the same than <x> >> 8.

```diff
-	uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);
+	uint256[] memory seenBidMap = new uint256[]((bidIndices.length >> 8)+1);
```
[SizeSealed.sol#L241](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L241)


```diff
-	uint256 bitmapIndex = bidIndex / 256;
-	uint256 bitmapIndex = bidIndex >> 8;
```
[SizeSealed.sol#L249](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L249)
	
## 6. Remove local variables that are used once
### Description
```diff
-	uint256 quoteBought = FixedPointMathLib.mulDivDown(baseAmount, a.data.lowestQuote, a.data.lowestBase);
-	uint256 refundedQuote = b.quoteAmount - quoteBought;
+	uint256 refundedQuote = b.quoteAmount - FixedPointMathLib.mulDivDown(baseAmount, a.data.lowestQuote, a.data.lowestBase);
```
### Lines in the code
[SizeSealed.sol#L377-L378](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L377-L378)
	