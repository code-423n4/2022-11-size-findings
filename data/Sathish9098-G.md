1) 

ECCMath.sol  #L27 

 In IF statement we don't want to check both conditions. Here using "||" operator so if first CONDITION true then we can skip all other conditions. Second condition only checked when the first condition is false. In this way can save gas fee 

CODE :

/// @notice calculates point^scalar
    /// @dev returns (1,1) if the ecMul failed or invalid parameters
    /// @return corresponding point
    function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {
        bytes memory data = abi.encode(point, scalar);
        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);
        (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(data);
        if (!res) return Point(1, 1);
        return abi.decode(ret, (Point));
    }

SizeSealed.sol #L144 #l187 #L426


 if (quoteAmount == 0 || quoteAmount == type(uint128).max || quoteAmount < a.params.minimumBidQuote) {
            revert InvalidBidAmount();
        }


ECCMath.Point memory pubKey = ECCMath.publicKey(privateKey);
        if (pubKey.x != a.params.pubKey.x || pubKey.y != a.params.pubKey.y || (pubKey.x == 1 && pubKey.y == 1)) {
            revert InvalidPrivateKey();
        }


 // Only allow bid cancellations while not finalized or in the reveal period
        if (block.timestamp >= a.timings.endTimestamp) {
            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {
                revert InvalidState();
            }
        }



PROOF OF CONCEPT :

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L27

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L144

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L187

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L426

|| OPERATOR LOGIC :

true + true = true
true + false= true
false + true =true
false + false=false 

SAME PROBLEM REPEATED 4 TIMES . IF WE SOLVE THIS CAN SAVE MORE VOLUME OF GAS

-------------------------------------------------------------------------------------------------------------------------------

2)

SizeSealed.sol #L302  #L244

In For loop we can use ++i Instead of i++ . ++i Consume less gas fee compared to i++ . There is difference in ++i and i++. 

CODE :

// Fill orders from highest price to lowest price
        for (uint256 i; i < bidIndices.length; i++) {
            uint256 bidIndex = bidIndices[i];
            EncryptedBid storage b = a.bids[bidIndex];


/ seenBidMap[0:len-1] should be full
        for (uint256 i; i < seenBidMap.length - 1; i++) {
            if (seenBidMap[i] != type(uint256).max) {
                revert InvalidState();
            }
        }

PROOF OF CONCEPT :

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244

----------------------------------------------------------------------------------------------------------------------------------------

3) 

SizeSealed.sol  #L258

When we using "&&" Operator if any one condition FALSE over all if statement become false . So if first condition FALSE then not needed to check second condition. It would cost more gas fee . Second condition only checked if first condition is TRUE. 

&& LOGIC :

false * fase=false
true*false=false
false *true=false
true * true = true 

CODE :

 // If the bidder public key isn't on the bn128 curve
            if (sharedPoint.x == 1 && sharedPoint.y == 1) continue;

PROOF OF CONCEPT :

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L258

--------------------------------------------------------------------------------------------------------------------------------------------

4)  SizeSealed.sol  #l294

In this code instead "data.filledBase += baseAmount" we can use   " data.filledBase=data.filledBase + baseAmount. += consumes more gas fee than normal addition. In this way we can reduce our gas fee 

CODE :

            b.filledBaseAmount = baseAmount;
            data.filledBase += baseAmount;
        }

PROOF OF CONCEPT :

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L294