### G01: PREFIX INCREMENT SAVE MORE GAS
#### problem
prefix increment ++i is more cheaper than postfix i++
#### prof
src/SizeSealed.sol, 244, b'        for (uint256 i; i < bidIndices.length; i++) {'
src/SizeSealed.sol, 302, b'        for (uint256 i; i < seenBidMap.length - 1; i++) {'



### G02: ++I/I++ SHOULD BE UNCHECKED{++I}/UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS
#### problem
The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop
#### prof
src/SizeSealed.sol, 84, b'        uint256 auctionId = ++currentAuctionId;'
src/SizeSealed.sol, 244, b'        for (uint256 i; i < bidIndices.length; i++) {'
src/SizeSealed.sol, 302, b'        for (uint256 i; i < seenBidMap.length - 1; i++) {'

### G03：ABI.ENCODE() IS LESS EFFICIENT THAN ABI.ENCODEPACKED()
#### prof
src/SizeSealed.sol, 467, b'        return keccak256(abi.encode(message));'
src/util/ECCMath.sol, 26, b'        bytes memory data = abi.encode(point, scalar);'
src/util/ECCMath.sol, 61, b'        return keccak256(abi.encode(point));'

### G04: USAGE OF UINTS/INTS SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD
#### problem
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
#### prof
src/SizeSealed.sol, 60, b'        if (timings.endTimestamp <= block.timestamp) {'
src/SizeSealed.sol, 63, b'        if (timings.startTimestamp >= timings.endTimestamp) {'
src/SizeSealed.sol, 63, b'        if (timings.startTimestamp >= timings.endTimestamp) {'
src/SizeSealed.sol, 66, b'        if (timings.endTimestamp > timings.vestingStartTimestamp) {'
src/SizeSealed.sol, 66, b'        if (timings.endTimestamp > timings.vestingStartTimestamp) {'
src/SizeSealed.sol, 69, b'        if (timings.vestingStartTimestamp > timings.vestingEndTimestamp) {'
src/SizeSealed.sol, 69, b'        if (timings.vestingStartTimestamp > timings.vestingEndTimestamp) {'
src/SizeSealed.sol, 72, b'        if (timings.cliffPercent > 1e18) {'
src/SizeSealed.sol, 72, b'        if (timings.cliffPercent > 1e18) {'
......
src/SizeSealed.sol, 289, b'            if (data.filledBase + baseAmount > data.totalBaseAmount) {'
src/SizeSealed.sol, 289, b'            if (data.filledBase + baseAmount > data.totalBaseAmount) {'
src/SizeSealed.sol, 289, b'            if (data.filledBase + baseAmount > data.totalBaseAmount) {'
src/SizeSealed.sol, 290, b'                baseAmount = data.totalBaseAmount - data.filledBase;'
src/SizeSealed.sol, 293, b'            b.filledBaseAmount = baseAmount;'
src/SizeSealed.sol, 294, b'            data.filledBase += baseAmount;'
src/SizeSealed.sol, 297, b'        if (data.previousQuotePerBase != FixedPointMathLib.mulDivDown(clearingQuote, type(uint128).max, clearingBase)) {'
src/SizeSealed.sol, 297, b'        if (data.previousQuotePerBase != FixedPointMathLib.mulDivDown(clearingQuote, type(uint128).max, clearingBase)) {'
src/SizeSealed.sol, 297, b'        if (data.previousQuotePerBase != FixedPointMathLib.mulDivDown(clearingQuote, type(uint128).max, clearingBase)) {'
src/SizeSealed.sol, 297, b'        if (data.previousQuotePerBase != FixedPointMathLib.mulDivDown(clearingQuote, type(uint128).max, clearingBase)) {'
src/SizeSealed.sol, 297, b'        if (data.previousQuotePerBase != FixedPointMathLib.mulDivDown(clearingQuote, type(uint128).max, clearingBase)) {'
src/SizeSealed.sol, 297, b'        if (data.previousQuotePerBase != FixedPointMathLib.mulDivDown(clearingQuote, type(uint128).max, clearingBase)) {'
src/SizeSealed.sol, 302, b'        for (uint256 i; i < seenBidMap.length - 1; i++) {'
src/SizeSealed.sol, 309, b'        if (seenBidMap[seenBidMap.length - 1] != (1 << (bidIndices.length % 256)) - 1) {'
src/SizeSealed.sol, 313, b'        if (data.filledBase > data.totalBaseAmount) {'
src/SizeSealed.sol, 313, b'        if (data.filledBase > data.totalBaseAmount) {'
src/SizeSealed.sol, 318, b'        if (data.totalBaseAmount != data.filledBase) {'
src/SizeSealed.sol, 318, b'        if (data.totalBaseAmount != data.filledBase) {'
src/SizeSealed.sol, 319, b'            uint128 unsoldBase = data.totalBaseAmount - data.filledBase;'
src/SizeSealed.sol, 319, b'            uint128 unsoldBase = data.totalBaseAmount - data.filledBase;'
src/SizeSealed.sol, 319, b'            uint128 unsoldBase = data.totalBaseAmount - data.filledBase;'
src/SizeSealed.sol, 319, b'            uint128 unsoldBase = data.totalBaseAmount - data.filledBase;'
src/SizeSealed.sol, 320, b'            a.params.totalBaseAmount = data.filledBase;'
src/SizeSealed.sol, 321, b'            SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), a.data.seller, unsoldBase);'
src/SizeSealed.sol, 321, b'            SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), a.data.seller, unsoldBase);'
src/SizeSealed.sol, 321, b'            SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), a.data.seller, unsoldBase);'
src/SizeSealed.sol, 325, b'        uint256 filledQuote = FixedPointMathLib.mulDivDown(clearingQuote, data.filledBase, clearingBase);'
src/SizeSealed.sol, 325, b'        uint256 filledQuote = FixedPointMathLib.mulDivDown(clearingQuote, data.filledBase, clearingBase);'
src/SizeSealed.sol, 325, b'        uint256 filledQuote = FixedPointMathLib.mulDivDown(clearingQuote, data.filledBase, clearingBase);'
src/SizeSealed.sol, 325, b'        uint256 filledQuote = FixedPointMathLib.mulDivDown(clearingQuote, data.filledBase, clearingBase);'
src/SizeSealed.sol, 327, b'        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), a.data.seller, filledQuote);'
src/SizeSealed.sol, 327, b'        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), a.data.seller, filledQuote);'
src/SizeSealed.sol, 339, b'        if (msg.sender != b.sender) {'
src/SizeSealed.sol, 343, b'        if (b.filledBaseAmount != 0) {'
src/SizeSealed.sol, 343, b'        if (b.filledBaseAmount != 0) {'
src/SizeSealed.sol, 343, b'        if (b.filledBaseAmount != 0) {'
src/SizeSealed.sol, 351, b'        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount);'
src/SizeSealed.sol, 351, b'        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount);'
src/SizeSealed.sol, 351, b'        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount);'
src/SizeSealed.sol, 361, b'        if (msg.sender != b.sender) {'
src/SizeSealed.sol, 365, b'        uint128 baseAmount = b.filledBaseAmount;'
src/SizeSealed.sol, 365, b'        uint128 baseAmount = b.filledBaseAmount;'
src/SizeSealed.sol, 365, b'        uint128 baseAmount = b.filledBaseAmount;'
src/SizeSealed.sol, 366, b'        if (baseAmount == 0) {'
src/SizeSealed.sol, 366, b'        if (baseAmount == 0) {'
src/SizeSealed.sol, 370, b'        uint128 baseTokensAvailable = tokensAvailableForWithdrawal(auctionId, baseAmount);'
src/SizeSealed.sol, 370, b'        uint128 baseTokensAvailable = tokensAvailableForWithdrawal(auctionId, baseAmount);'
src/SizeSealed.sol, 370, b'        uint128 baseTokensAvailable = tokensAvailableForWithdrawal(auctionId, baseAmount);'
src/SizeSealed.sol, 371, b'        baseTokensAvailable = baseTokensAvailable - b.baseWithdrawn;'
src/SizeSealed.sol, 373, b'        b.baseWithdrawn += baseTokensAvailable;'
src/SizeSealed.sol, 376, b'        if (b.quoteAmount != 0) {'
src/SizeSealed.sol, 376, b'        if (b.quoteAmount != 0) {'
src/SizeSealed.sol, 376, b'        if (b.quoteAmount != 0) {'
src/SizeSealed.sol, 377, b'            uint256 quoteBought = FixedPointMathLib.mulDivDown(baseAmount, a.data.lowestQuote, a.data.lowestBase);'
src/SizeSealed.sol, 377, b'            uint256 quoteBought = FixedPointMathLib.mulDivDown(baseAmount, a.data.lowestQuote, a.data.lowestBase);'
src/SizeSealed.sol, 377, b'            uint256 quoteBought = FixedPointMathLib.mulDivDown(baseAmount, a.data.lowestQuote, a.data.lowestBase);'
src/SizeSealed.sol, 377, b'            uint256 quoteBought = FixedPointMathLib.mulDivDown(baseAmount, a.data.lowestQuote, a.data.lowestBase);'
src/SizeSealed.sol, 377, b'            uint256 quoteBought = FixedPointMathLib.mulDivDown(baseAmount, a.data.lowestQuote, a.data.lowestBase);'
src/SizeSealed.sol, 377, b'            uint256 quoteBought = FixedPointMathLib.mulDivDown(baseAmount, a.data.lowestQuote, a.data.lowestBase);'
src/SizeSealed.sol, 378, b'            uint256 refundedQuote = b.quoteAmount - quoteBought;'
src/SizeSealed.sol, 378, b'            uint256 refundedQuote = b.quoteAmount - quoteBought;'
src/SizeSealed.sol, 379, b'            b.quoteAmount = 0;'
src/SizeSealed.sol, 381, b'            SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, refundedQuote);'
src/SizeSealed.sol, 384, b'        SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), msg.sender, baseTokensAvailable);'
src/SizeSealed.sol, 384, b'        SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), msg.sender, baseTokensAvailable);'
src/SizeSealed.sol, 393, b'        if (msg.sender != a.data.seller) {'
src/SizeSealed.sol, 398, b'        if (a.data.lowestQuote != type(uint128).max) {'
src/SizeSealed.sol, 398, b'        if (a.data.lowestQuote != type(uint128).max) {'
src/SizeSealed.sol, 398, b'        if (a.data.lowestQuote != type(uint128).max) {'
src/SizeSealed.sol, 398, b'        if (a.data.lowestQuote != type(uint128).max) {'
src/SizeSealed.sol, 398, b'        if (a.data.lowestQuote != type(uint128).max) {'
src/SizeSealed.sol, 405, b'        a.timings.endTimestamp = type(uint32).max;'
src/SizeSealed.sol, 409, b'        SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), msg.sender, a.params.totalBaseAmount);'
src/SizeSealed.sol, 409, b'        SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), msg.sender, a.params.totalBaseAmount);'
src/SizeSealed.sol, 409, b'        SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), msg.sender, a.params.totalBaseAmount);'
src/SizeSealed.sol, 420, b'        if (msg.sender != b.sender) {'
src/SizeSealed.sol, 425, b'        if (block.timestamp >= a.timings.endTimestamp) {'
src/SizeSealed.sol, 425, b'        if (block.timestamp >= a.timings.endTimestamp) {'
src/SizeSealed.sol, 426, b'            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {'
src/SizeSealed.sol, 426, b'            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {'
src/SizeSealed.sol, 426, b'            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {'
src/SizeSealed.sol, 426, b'            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {'
src/SizeSealed.sol, 426, b'            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {'
src/SizeSealed.sol, 426, b'            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {'
src/SizeSealed.sol, 426, b'            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {'
src/SizeSealed.sol, 426, b'            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {'
src/SizeSealed.sol, 426, b'            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {'
src/SizeSealed.sol, 439, b'        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount);'
src/SizeSealed.sol, 439, b'        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount);'
src/SizeSealed.sol, 439, b'        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount);'
src/SizeSealed.sol, 463, b'        return CommonTokenMath.tokensAvailableAtTime(\n            a.timings.vestingStartTimestamp,\n            a.timings.vestingEndTimestamp,\n            uint32(block.timestamp),\n            a.timings.cliffPercent,\n            baseAmount\n        );'
src/SizeSealed.sol, 457, b'        return CommonTokenMath.tokensAvailableAtTime('
src/SizeSealed.sol, 458, b'            a.timings.vestingStartTimestamp,'
src/SizeSealed.sol, 458, b'            a.timings.vestingStartTimestamp,'
src/SizeSealed.sol, 459, b'            a.timings.vestingEndTimestamp,'
src/SizeSealed.sol, 459, b'            a.timings.vestingEndTimestamp,'
src/SizeSealed.sol, 460, b'            uint32(block.timestamp),'
src/SizeSealed.sol, 460, b'            uint32(block.timestamp),'
src/SizeSealed.sol, 461, b'            a.timings.cliffPercent,'
src/SizeSealed.sol, 461, b'            a.timings.cliffPercent,'
src/SizeSealed.sol, 462, b'            baseAmount'
src/SizeSealed.sol, 471, b'        return bytes32(abi.encodePacked(baseAmount, salt));'
src/util/CommonTokenMath.sol, 54, b'        if (currentTime > vestingEnd) {'
src/util/CommonTokenMath.sol, 54, b'        if (currentTime > vestingEnd) {'
src/util/CommonTokenMath.sol, 56, b'        } else if (currentTime <= vestingStart) {'
src/util/CommonTokenMath.sol, 56, b'        } else if (currentTime <= vestingStart) {'
src/util/CommonTokenMath.sol, 60, b'            uint256 cliffAmount = FixedPointMathLib.mulDivDown(baseAmount, cliffPercent, 1e18);'
src/util/CommonTokenMath.sol, 60, b'            uint256 cliffAmount = FixedPointMathLib.mulDivDown(baseAmount, cliffPercent, 1e18);'
src/util/CommonTokenMath.sol, 60, b'            uint256 cliffAmount = FixedPointMathLib.mulDivDown(baseAmount, cliffPercent, 1e18);'
src/util/CommonTokenMath.sol, 60, b'            uint256 cliffAmount = FixedPointMathLib.mulDivDown(baseAmount, cliffPercent, 1e18);'
src/util/CommonTokenMath.sol, 67, b'            return uint128(\n                cliffAmount\n                    + FixedPointMathLib.mulDivDown(\n                        baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart\n                    )\n            );'
src/util/CommonTokenMath.sol, 62, b'            return uint128('
src/util/CommonTokenMath.sol, 64, b'                    + FixedPointMathLib.mulDivDown('
src/util/CommonTokenMath.sol, 65, b'                        baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart'
src/util/CommonTokenMath.sol, 65, b'                        baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart'
src/util/CommonTokenMath.sol, 65, b'                        baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart'
src/util/CommonTokenMath.sol, 65, b'                        baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart'
src/util/CommonTokenMath.sol, 65, b'                        baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart'
src/util/CommonTokenMath.sol, 65, b'                        baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart'
src/util/CommonTokenMath.sol, 65, b'                        baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart'
src/util/CommonTokenMath.sol, 57, b"            return 0; // If cliff hasn't been triggered yet, bidder receives no tokens"
src/util/CommonTokenMath.sol, 55, b'            return baseAmount; // If vesting is over, bidder is owed all tokens'
src/util/ECCMath.sol, 19, b'        return ecMul(Point(GX, GY), privateKey);'
src/util/ECCMath.sol, 19, b'        return ecMul(Point(GX, GY), privateKey);'
src/util/ECCMath.sol, 19, b'        return ecMul(Point(GX, GY), privateKey);'
src/util/ECCMath.sol, 26, b'        bytes memory data = abi.encode(point, scalar);'
src/util/ECCMath.sol, 27, b'        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);'
src/util/ECCMath.sol, 27, b'        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);'
src/util/ECCMath.sol, 27, b'        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);'
src/util/ECCMath.sol, 27, b'        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);'
src/util/ECCMath.sol, 27, b'        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);'
src/util/ECCMath.sol, 28, b'        (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(data);'
src/util/ECCMath.sol, 29, b'        if (!res) return Point(1, 1);'
src/util/ECCMath.sol, 29, b'        if (!res) return Point(1, 1);'
src/util/ECCMath.sol, 29, b'        if (!res) return Point(1, 1);'
src/util/ECCMath.sol, 29, b'        if (!res) return Point(1, 1);'
src/util/ECCMath.sol, 30, b'        return abi.decode(ret, (Point));'
src/util/ECCMath.sol, 30, b'        return abi.decode(ret, (Point));'
src/util/ECCMath.sol, 42, b'        Point memory sharedPoint = ecMul(encryptToPub, encryptWithPriv);'
src/util/ECCMath.sol, 42, b'        Point memory sharedPoint = ecMul(encryptToPub, encryptWithPriv);'
src/util/ECCMath.sol, 42, b'        Point memory sharedPoint = ecMul(encryptToPub, encryptWithPriv);'
src/util/ECCMath.sol, 43, b'        bytes32 sharedKey = hashPoint(sharedPoint);'
src/util/ECCMath.sol, 43, b'        bytes32 sharedKey = hashPoint(sharedPoint);'
src/util/ECCMath.sol, 45, b'        buyerPub = publicKey(encryptWithPriv);'
src/util/ECCMath.sol, 56, b'        return encryptedMessage ^ hashPoint(sharedPoint);'
src/util/ECCMath.sol, 56, b'        return encryptedMessage ^ hashPoint(sharedPoint);'
