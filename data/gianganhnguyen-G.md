# 1. [G-1] For loops: increments in for loop can be uncheck to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block

    File src/SizeSealed.sol, line 244:     for (uint256 i; i < bidIndices.length; i++) {
    File src/SizeSealed.sol, line 302:     for (uint256 i; i < seenBidMap.length - 1; i++) {

The code would go from:

    for (uint256 i; i < numIterations; i++) { 
      ...
    }

to:

    for (uint256 i; i < numIterations;) { 
      ...
      unchecked { ++i; }  
    }

# 2. [G-2] Variables: Cache read variables in memory will save gas

    File src/SizeSealed.sol, line 337, 359, 418, 456:     Auction storage a = idToAuction[auctionId]; // I suggest code replace: Auction memory a = idToAuction[auctionId];

# 3. [G-3] Parameter: If we are not modifying the passed parameter we should pass it as `calldata` because `calldata` is more gas efficient than `memory`.

I suggest using `calldata` instead of `memory` here:

    File src/SizeSealed.sol, line 217:     function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)

    File src/util/ECCMath.sol, line 25:     function ecMul(Point memory point, uint256 scalar)
    File src/util/ECCMath.sol, line 37:     function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)
    File src/util/ECCMath.sol, line 51:     function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)
    File src/util/ECCMath.sol, line 60:     function hashPoint(Point memory point)

# 4. [G-4] Arithmetics: uncheck blocks for arithmetics operations that can't underflow/overflow

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn't possible, some gas can be saved by using an `unchecked` block.

I suggest wrapping with an `unchecked` block here:

    File src/SizeSealed.sol, line 84:       uint256 auctionId = ++currentAuctionId;
    File src/SizeSealed.sol, line 249:     uint256 bitmapIndex = bidIndex / 256;
    File src/SizeSealed.sol, line 251:     uint256 indexBit = 1 << (bidIndex % 256);
    File src/SizeSealed.sol, line 378:     uint256 refundedQuote = b.quoteAmount - quoteBought;