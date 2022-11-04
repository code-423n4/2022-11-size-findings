# Report


## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1] | Loop can be more optimizable| 2 |
| [GAS-2] | INCREMENTS can be UNCHECKED | 1 |
| [GAS-3] | Gas can saved by simply reordering the code in function bid() | 1 |
| [GAS-4] | auctionParams.baseToken Should cached in memory | 3 |
| [GAS-5] | <x> += <y> costs more gas than <x> = <x> + <y> for state variables | 2 |
| [GAS-6] | Function that guaranteed to revert when called by normal user can be marked payable | 1 |
| [GAS-7] | Redundant type casting | 1 |
| [GAS-8] | abi.encode() is less efficient than abi.encodePacked() | 3 |



### [GAS-01] Loop can be more optimizable

. Should first cached array.length( like bidIndices.length ) into memory and then use in loop condition check
. Should use ++i instead i++
. Should uncheck ++i


*Instances (2)*:
```solidity
File: src/SizeSealed.sol

244:         https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244
302:         https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302

```


### [GAS-02] INCREMENTS can be UNCHECKED
Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block: https://docs.soliditylang.org/en/v0.8.10/control-structures.html#checked-or-unchecked-arithmetic 


*Instances (1)*:
```solidity
File: src/SizeSealed.sol

84:         https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L84

```

### [GAS-03] Gas can saved by simply reordering the code in function bid()
Inside bid() function first verification of markel proof occurs and then caller(msg.sender) is seller or not checks occure

If simply we reorder that means making first check for seller then after check for markel proof, transaction will reverted eairlyer when it is called by seller, this way we can save extra gas that goes for verifing markel proof. 

*Instances (1)*:
```solidity
File: src/SizeSealed.sol

131-142:         https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L131-L142

```

### [GAS-04] auctionParams.baseToken Should cached in memory

*Instances (3)*:
```solidity
File: src/SizeSealed.sol

90-105:         https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L96-L105

```

### [GAS-05] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

*Instances (2)*:
```solidity
File: src/SizeSealed.sol

294:         https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L294
373:         https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L373
```

### [GAS-06] Function that guaranteed to revert when called by normal user can be marked payable 

Functions that include condition checks and are callable by specific User(like seller), the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. 


*Instances (1)*:
```solidity
File: src/SizeSealed.sol

391:         https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L391-L410
```

### [GAS-07] Redundant type casting
In function computeMessage(), ```solidity abi.encodePacked(baseAmount, salt); ``` already returns result in bytes32
No need to explicitly convert that into bytes32

*Instances (1)*:
```solidity
File: src/SizeSealed.sol

471:         https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L471
```

### [GAS-08] abi.encode() is less efficient than abi.encodePacked()
*Instances (3)*:
```solidity
File: src/SizeSealed.sol

467:         https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L467
```
```solidity
File: src/util/ECCMath.sol

26:          https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L26
61:          https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L61

```




