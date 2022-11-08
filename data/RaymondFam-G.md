## Non-strict inequalities are cheaper than strict ones
In EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =). As an example, consider replacing >= with the strict counterpart > in the following line of code:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L63
```
        if (timings.startTimestamp > timings.endTimestamp - 1) {
```
Similarly, as an example, consider replacing <= with the strict counterpart < in the following line of code:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L60

```
        if (timings.endTimestamp < block.timestamp + 1) {
```

## `||` Costs Less Gas Than Its Equivalent `&&`
Rule of thumb: `(x && y)` is `(!(!x || !y))`

Even with the 10k Optimizer enabled: `||`, OR conditions cost less than their equivalent `&&`, AND conditions.

As an example, the following code line may be rewritten as:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L258

```
            if (!(sharedPoint.x != 1 || sharedPoint.y != 1)) continue;
```
## calldata and memory
When running a function we could pass the function parameters as calldata or memory for variables such as strings, bytes, structs, arrays etc. If we are not modifying the passed parameter, we should pass it as calldata because calldata is more gas efficient than memory.

Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. However, it alleviates the compiler from the `abi.decode()` step that copies each index of the calldata to the memory index, bypassing the cost of 60 gas on each iteration. Here are some of the instances entailed:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217

## Use Storage Instead of Memory for Structs/Arrays
A storage pointer is cheaper since copying a state struct in memory would incur as many SLOADs and MSTOREs as there are slots. In another words, this causes all fields of the struct/array to be read from storage, incurring a Gcoldsload (2100 gas) for each field of the struct/array, and then further incurring an additional MLOAD rather than a cheap stack read. As such, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables will be much cheaper, involving only Gcoldsload for all associated field reads. Read the whole struct/array into a memory variable only when it is being returned by the function, passed into a function that requires memory, or if the array/struct is being read from another memory array/struct. Here are some of the instances entailed:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L148
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L186
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L231
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L256

## Ternary Over `if ... else`
Using ternary operator instead of the if else statement saves gas. For instance the following code block may be rewritten as:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L272-L276

```
    quotePerBase == data.previousQuotePerBase
        ? {
            if (data.previousIndex > bidIndex) revert InvalidSorting();
        }
        : {
           revert InvalidSorting();
        }
```
## Private/Internal Function Embedded Modifier Reduces Contract Size
Consider having the logic of a modifier embedded through an internal or private (if no contracts inheriting) function to reduce contract size if need be. For instance, the following instance of modifier may be rewritten as follows:

```
    function _atState(Auction storage a, States _state) private view {
        if (block.timestamp < a.timings.startTimestamp) {
            if (_state != States.Created) revert InvalidState();
        } else if (block.timestamp < a.timings.endTimestamp) {
            if (_state != States.AcceptingBids) revert InvalidState();
        } else if (a.data.lowestQuote != type(uint128).max) {
            if (_state != States.Finalized) revert InvalidState();
        } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
            if (_state != States.RevealPeriod) revert InvalidState();
        } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
            if (_state != States.Voided) revert InvalidState();
        } else {
            revert();
        }
    }

    modifier atState(Auction storage a, States _state) {
        _atState(a, _state);
        _;
    }
```
## Function Order Affects Gas Consumption
The order of function will also have an impact on gas consumption. Because in smart contracts, there is a difference in the order of the functions. Each position will have an extra 22 gas. The order is dependent on method ID. So, if you rename the frequently accessed function to more early method ID, you can save gas cost. Please visit the following site for further information:

https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Activate the Optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
  solidity: {
    version: "0.8.17",
    settings: {
      optimizer: {
        enabled: true,
        runs: 1000,
      },
    },
  },
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:

for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI

Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past . A high-severity bug in the emscripten -generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

## `uint256` Can be Cheaper Than `uint8` and Other Unsigned Integer Type of Smaller Bit Size
When dealing with function arguments or memory values, there is no inherent benefit because the compiler does not pack these values. Your contract’s gas usage may be higher because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. The EVM needs to properly enforce the limits of this smaller type.

It is only more efficient when you can pack variables of uint8, uint16, uint32, uint64, ... into the same storage slot with other neighboring variables smaller than 32 bytes. Here are some of the instances entailed:

https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L48-L50

## Unchecked SafeMath Saves Gas
"Checked" math, which is default in ^0.8.0 is not free. The compiler will add some overflow checks, somehow similar to those implemented by `SafeMath`. While it is reasonable to expect these checks to be less expensive than the current `SafeMath`, one should keep in mind that these checks will increase the cost of "basic math operation" that were not previously covered. This particularly concerns variable increments in for loops. When no arithmetic overflow/underflow is going to happen, `unchecked { ++i ;}` to use the previous wrapping behavior further saves gas just as in the for loop below as an example:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302-L305

```
        for (uint256 i; i < seenBidMap.length - 1; ) {
          if (seenBidMap[i] != type(uint256).max) {
              revert InvalidState();
          }

          unchecked {
              ++i;
          }
        }
```
## `abi.encode()` Costs More Gas Than `abi.encodePacked()`
Changing `abi.encode()` to `abi.encodePacked()` can save gas considering the former pads extra null bytes at the end of the call data, which is unnecessary. Please visit the following the link delineating how `abi.encodePacked()` is more gas efficient in general:

https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison

Here is one of the instances entailed:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L467

## Use of Named Returns for Local Variables Saves Gas
You can have further advantages in term of gas cost by simply using named return values as temporary local variable. As an example, the following instance of code block can refactored as follows:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L466-L468

```
    function computeCommitment(bytes32 message) public pure returns (bytes32 commitmentHash) {
        commitmentHash = keccak256(abi.encode(message));
    }
```
## += and -= Costs More Gas
`+=` generally costs 22 more gas than writing out the assigned equation explicitly. The amount of gas wasted can be quite sizable when repeatedly operated in a loop. As an example, the following line of code could be rewritten as:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L294

```
            data.filledBase = data.filledBase + baseAmount;
```
## Calling Internal functions Is Cheaper
From inside a smart contract, calling its internal functions is cheaper than calling its public functions, because when you call a public function, all the parameters are again copied into memory and passed to that function. By contrast, when you call an internal function, references of those parameters are passed and they are not copied into memory again. This saves a bit of gas, especially when the parameters are big.

As such, `finalize()` should have its visibility changed to `internal` since calling it internally is the only viable way to have `a.data.privKey` updated.