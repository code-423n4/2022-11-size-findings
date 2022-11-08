## Extract Sanity Checks from Loop

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L240-L311

The code linked above and listed at the end of this issue description can be reorganized to require significantly less gas for typical inputs.

There should be two loops. The first should look something like this:

    // Bitmap of all the bid indices that have been processed
    uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);


    // Fill orders from highest price to lowest price
    for (uint256 i; i < bidIndices.length; i++) {
        uint256 bidIndex = bidIndices[i];
        EncryptedBid storage b = a.bids[bidIndex];


        // Verify this bid index hasn't been seen before
        uint256 bitmapIndex = bidIndex / 256;
        uint256 bitMap = seenBidMap[bitmapIndex];
        uint256 indexBit = 1 << (bidIndex % 256);
        if (bitMap & indexBit == 1) revert InvalidState();
        seenBidMap[bitmapIndex] = bitMap | indexBit;

        // Require that bids are passed in descending price
        uint256 quotePerBase = FixedPointMathLib.mulDivDown(b.quoteAmount, type(uint128).max, baseAmount);
        if (quotePerBase >= data.previousQuotePerBase) {
            // If last bid was the same price, make sure we filled the earliest bid first
            if (quotePerBase == data.previousQuotePerBase) {
                if (data.previousIndex > bidIndex) revert InvalidSorting();
            } else {
                revert InvalidSorting();
            }
        }
    }

Then we can change the 'continue's in the following code

    // Only fill if above reserve price
    if (quotePerBase < data.reserveQuotePerBase) continue;


    // Auction has been fully filled
    if (data.filledBase == data.totalBaseAmount) continue;
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L279-L283

to 'break's and skip going through all the heavy lifting (decrypting..) for a potentially significant part of the bids.

Note that the end result, including all checks, stays the same.

Listing of the original code:

***

    // Bitmap of all the bid indices that have been processed
    uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);


    // Fill orders from highest price to lowest price
    for (uint256 i; i < bidIndices.length; i++) {
        uint256 bidIndex = bidIndices[i];
        EncryptedBid storage b = a.bids[bidIndex];


        // Verify this bid index hasn't been seen before
        uint256 bitmapIndex = bidIndex / 256;
        uint256 bitMap = seenBidMap[bitmapIndex];
        uint256 indexBit = 1 << (bidIndex % 256);
        if (bitMap & indexBit == 1) revert InvalidState();
        seenBidMap[bitmapIndex] = bitMap | indexBit;


        // G^k1^k2 == G^k2^k1
        ECCMath.Point memory sharedPoint = ECCMath.ecMul(b.pubKey, sellerPriv);
        // If the bidder public key isn't on the bn128 curve
        if (sharedPoint.x == 1 && sharedPoint.y == 1) continue;


        bytes32 decryptedMessage = ECCMath.decryptMessage(sharedPoint, b.encryptedMessage);
        // If the bidder didn't faithfully submit commitment or pubkey
        // Or the bid was cancelled
        if (computeCommitment(decryptedMessage) != b.commitment) continue;


        // First 128 bits are the base amount, last are random salt
        uint128 baseAmount = uint128(uint256(decryptedMessage >> 128));


        // Require that bids are passed in descending price
        uint256 quotePerBase = FixedPointMathLib.mulDivDown(b.quoteAmount, type(uint128).max, baseAmount);
        if (quotePerBase >= data.previousQuotePerBase) {
            // If last bid was the same price, make sure we filled the earliest bid first
            if (quotePerBase == data.previousQuotePerBase) {
                if (data.previousIndex > bidIndex) revert InvalidSorting();
            } else {
                revert InvalidSorting();
            }
        }


        // Only fill if above reserve price
        if (quotePerBase < data.reserveQuotePerBase) continue;


        // Auction has been fully filled
        if (data.filledBase == data.totalBaseAmount) continue;


        data.previousQuotePerBase = quotePerBase;
        data.previousIndex = bidIndex;


        // Fill the remaining unfilled base amount
        if (data.filledBase + baseAmount > data.totalBaseAmount) {
            baseAmount = data.totalBaseAmount - data.filledBase;
        }


        b.filledBaseAmount = baseAmount;
        data.filledBase += baseAmount;
    }


    if (data.previousQuotePerBase != FixedPointMathLib.mulDivDown(clearingQuote, type(uint128).max, clearingBase)) {
        revert InvalidCalldata();
    }


    // seenBidMap[0:len-1] should be full
    for (uint256 i; i < seenBidMap.length - 1; i++) {
        if (seenBidMap[i] != type(uint256).max) {
            revert InvalidState();
        }
    }


    // seenBidMap[-1] should only have the last N bits set
    if (seenBidMap[seenBidMap.length - 1] != (1 << (bidIndices.length % 256)) - 1) {
        revert InvalidState();
    }
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L240-L311

## Redundant Check

In the following code the check for 'quoteAmount == 0' is included in 'quoteAmount < a.params.minimumBidQuote' and can be removed.
    
    if (quoteAmount == 0 || quoteAmount == type(uint128).max || quoteAmount < a.params.minimumBidQuote) {
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L144

