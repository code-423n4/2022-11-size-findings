GA1: https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L31
The comparison operator should be ``<=``. 

GA2: https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L69
The comparison operator should be ``>=``. 

GA3: https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L349
emit BidRefund(auctionId, bidIndex);
should inlude the bidder's address in the event