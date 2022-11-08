# Low
## L01 ineffective check for duplicate bids in `finalize()`

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L252

 The check 
```solidity
if (bitMap & indexBit == 1) revert InvalidState();
```
 only checks if the first bid of each bitMap has been seen before.
 The check should be 
```solidity
if (bitMap & indexBit == indexBit) revert InvalidState();
```

 Luckily there is a [check](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L301-L311) that verifies all the entries of the `bidIndices` parameter have been processed, which together with the [check](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L227) to see if the number of bids received for the auction == the `bidIndices`  length means this bug is not exploitable.

## L02 'Seller cannot submit bid' check not effective

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L140-L142
 The check in `bid()` to rule out the seller submitting a bid is easily circumvented by the seller using another address. As this part of the code gives a false sense os security to the reader of the code it is best to remove it.
 
## L03 `clearingQuote` and `clearingBase` as parameters of  `finalize()` serve no purpose

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L325

 Currently the  `clearingQuote` and `clearingBase` are used as  parameters of  `finalize()`, but at the end they are [required](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L297) to correspond with the last `previousQuotePerBase` value.  
 
 This does however open up for an attack vector using irregular values for `clearingQuote` and `clearingBase` which might cause rounding errors when used for payout as that [calculation](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L325) does not use the `type(uint128).max` precision as does the [calulation](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L269) for the `previousQuotePerBase`. 
 Although no exploitable use of these parameters has been found it is probably better to remove them as parameters and just take the last value of `previousQuotePerBase` to [calculate](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L325) the `filledQuote`.
 
 Note: for readability it is also advisable to use a `PRECISION` constant instead of the `type(uint128).max`
 

# Non-critical
## NC  Natspec errors
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L40


- No Natspec for [EncryptedBid](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L40) structure
- No Natspec for [AuctionData](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L63) struct
- No Natspec for [Acution](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L86) struct
- [`merkleRoot`](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L82) not in Natspec for `AuctionParameters` struct.

