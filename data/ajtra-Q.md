## Non Critical
1. NC01 - Event is missing indexed fields
2. NC02 - NatSpec incomplete
3. NC03 - Should not use magic number
4. NC04 - Not using the named return variables when a function returns in anyware is confusing
5. NC05 - The struct should be at the begining of the contract

# Non Critical
## NC01 - Event is missing indexed fields
### Description
Index event fields make the field more quickly accessible to off-chain tools that parse events. 
However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (threefields). 
Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. 
If there are fewer than three fields, all of the fields should be indexed.

### Lines in the code
[ISizeSealed.sol#L97-L122](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L97-L122)

## NC02 - NatSpec incomplete
### Description
NatSpec is incomplete in many functions. It's a best practice to complete it to a major understanding. 

### Lines in the code
#### Example in the code where is missing the return in NatSpec
[ECCMath.sol#L18](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L18)
[ECCMath.sol#L37](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L37)
[ECCMath.sol#L51](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L51)
[ECCMath.sol#L60](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L60)

## NC03 - Use constant instead of magic number
### Description
Replace the magic numbers for a constant that describe the meaning of it.

### Lines in the code
[SizeSealed.sol#L157](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L157)
[SizeSealed.sol#L241](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L241)
[SizeSealed.sol#L249](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L249)
[SizeSealed.sol#L251](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L251)
[SizeSealed.sol#L309](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L309)

## NC04 - Not using the named return variables when a function returns in anyware is confusing
[ECCMath.sol#L51](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L51)
[SizeSealed.sol#L451](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L451)

## NC05 - The struct should be at the begining of the contract
### Description
To a best understanding the struct definition should be at the begining of the contract.

### Lines in the code
[SizeSealed.sol#L203-L209](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L203-L209)
