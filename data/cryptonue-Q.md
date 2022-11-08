# [NC] Missing indexed in event

Indexed events are easier and faster to find in the transaction log. So it's better to use indexed in event. the `ISizeSealed.sol` events aren't using index

```solidity
File: ISizeSealed.sol
096: 
097:     event AuctionCreated(
098:         uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
099:     );
100: 
101:     event AuctionCancelled(uint256 auctionId);
102: 
103:     event Bid(
104:         address sender,
105:         uint256 auctionId,
106:         uint256 bidIndex,
107:         uint128 quoteAmount,
108:         bytes32 commitment,
109:         ECCMath.Point pubKey,
110:         bytes32 encryptedMessage,
111:         bytes encryptedPrivateKey
112:     );
113: 
114:     event BidCancelled(uint256 auctionId, uint256 bidIndex);
115: 
116:     event RevealedKey(uint256 auctionId, uint256 privateKey);
117: 
118:     event AuctionFinalized(uint256 auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);
119: 
120:     event BidRefund(uint256 auctionId, uint256 bidIndex);
121: 
122:     event Withdrawal(uint256 auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);
```
# [NC] Design of bidder flow is not efficient

Currently when a user bid X amount, they bid with sending their token also X amount. At some point, he want to bid a higher amount Y, (which Y > X), so he need to send Y amount. To make it efficient and use smaller index, if a user want to increase their bid, they can just update the previous bid value. In this case if user want to bid Y, they only need to send Y-X amount.