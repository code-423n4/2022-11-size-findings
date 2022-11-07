## Summary<a name="Summary">

### Gas Optimizations
| |Issue|Contexts|
|-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | `++i`/`i++` Should Be `unchecked{++i}`/`unchecked{i++}` When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops | 2 |
| [GAS&#x2011;2](#GAS&#x2011;2) | `abi.encode()` is less efficient than `abi.encodepacked()` | 3 |
| [GAS&#x2011;3](#GAS&#x2011;3) | Use calldata instead of memory for function parameters | 1 |
| [GAS&#x2011;4](#GAS&#x2011;4) | Public Functions To External | 1 |
| [GAS&#x2011;5](#GAS&#x2011;5) | Optimize names to save gas | 1 |

Total: 8 contexts over 5 issues

## Gas Optimizations


### <a href="#Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> `++i`/`i++` Should Be `unchecked{++i}`/`unchecked{i++}` When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas PER LOOP

#### <ins>Proof Of Concept</ins>


```
244: for (uint256 i; i < bidIndices.length; i++) {
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/SizeSealed.sol#L244

```
302: for (uint256 i; i < seenBidMap.length - 1; i++) {
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/SizeSealed.sol#L302





### <a href="#Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> `abi.encode()` is less efficient than `abi.encodepacked()`

See for more information: https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison 

#### <ins>Proof Of Concept</ins>


```
467: return keccak256(abi.encode(message));
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/SizeSealed.sol#L467

```
26: bytes memory data = abi.encode(point, scalar);
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/util/ECCMath.sol#L26

```
61: return keccak256(abi.encode(point));
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/util/ECCMath.sol#L61






### <a href="#Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> Use calldata instead of memory for function parameters

In some cases, having function arguments in calldata instead of
memory is more optimal.

Consider the following generic example:
```
contract C {
	function add(uint[] memory arr) external returns (uint sum) {
		uint length = arr.length;
		for (uint i = 0; i < arr.length; i++) {
		    sum += arr[i];
		}
	}
}
```
In the above example, the dynamic array arr has the storage location
memory. When the function gets called externally, the array values are
kept in calldata and copied to memory during ABI decoding (using the
opcode calldataload and mstore). And during the for loop, arr[i]
accesses the value in memory using a mload. However, for the above
example this is inefficient. Consider the following snippet instead:
```
contract C {
	function add(uint[] calldata arr) external returns (uint sum) {
		uint length = arr.length;
		for (uint i = 0; i < arr.length; i++) {
		    sum += arr[i];
		}
	}
}
```
In the above snippet, instead of going via memory, the value is directly
read from calldata using calldataload. That is, there are no
intermediate memory operations that carries this value.

Gas savings: In the former example, the ABI decoding begins with
copying value from calldata to memory in a for loop. Each iteration
would cost at least 60 gas. In the latter example, this can be
completely avoided. This will also reduce the number of instructions and
therefore reduces the deploy time cost of the contract.

In short, use calldata instead of memory if the function argument
is only read.

Note that in older Solidity versions, changing some function arguments
from memory to calldata may cause "unimplemented feature error".
This can be avoided by using a newer (0.8.*) Solidity compiler.

Examples
Note: The following pattern is prevalent in the codebase:
```
function f(bytes memory data) external {
	(...) = abi.decode(data, (..., types, ...));
}
```
Here, changing to bytes calldata will decrease the gas. The total
savings for this change across all such uses would be quite
significant.

#### <ins>Proof Of Concept</ins>


```
function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
        public
        atState(idToAuction[auctionId], States.RevealPeriod)
    {
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/SizeSealed.sol#L217






### <a href="#Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> Public Functions To External

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

#### <ins>Proof Of Concept</ins>


```
function computeCommitment(bytes32 message) public pure returns (bytes32) {
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/SizeSealed.sol#L466




### <a href="#Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> Optimize names to save gas

`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://github.com/enzosv/solidity-optimize-name) link for an example of how it works. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

#### <ins>Proof Of Concept</ins>

```
File: \2022-11-size\src\SizeSealed.sol
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/SizeSealed.sol






