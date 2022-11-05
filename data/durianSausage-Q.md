### N01: EVENT IS MISSING INDEXED FIELDS
#### prof
src/interfaces/ISizeSealed.sol, 99, b'    event AuctionCreated(\n        uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey\n    );'
src/interfaces/ISizeSealed.sol, 101, b'    event AuctionCancelled(uint256 auctionId);'
src/interfaces/ISizeSealed.sol, 112, b'    event Bid(\n        address sender,\n        uint256 auctionId,\n        uint256 bidIndex,\n        uint128 quoteAmount,\n        bytes32 commitment,\n        ECCMath.Point pubKey,\n        bytes32 encryptedMessage,\n        bytes encryptedPrivateKey\n    );'
src/interfaces/ISizeSealed.sol, 114, b'    event BidCancelled(uint256 auctionId, uint256 bidIndex);'
src/interfaces/ISizeSealed.sol, 116, b'    event RevealedKey(uint256 auctionId, uint256 privateKey);'
src/interfaces/ISizeSealed.sol, 118, b'    event AuctionFinalized(uint256 auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);'
src/interfaces/ISizeSealed.sol, 120, b'    event BidRefund(uint256 auctionId, uint256 bidIndex);'
src/interfaces/ISizeSealed.sol, 122, b'    event Withdrawal(uint256 auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);'