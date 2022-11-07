#### No. 1:

It seems that it is possible to bid on a cancelled auction. It should not be allowed. 
So, the code on line 405 is not correct:
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L405

It can be easily solved by setting `a.timings.endTimestamp = 0`, in this case the bidding on a cancelled auction will be reverted:
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L31

Moreover, the previous bidders will be able to cancel their bid and receive their funds. Note that the condition `if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours)` is false, because `a.data.lowestQuote = type(uint128).max` for an unfinalized auction.


