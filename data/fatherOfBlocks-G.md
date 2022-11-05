**SizeSealed**
- L60/63/66/69/96/99/102/227/241/244/302/309 - When a parameter is used multiple times and has to be looked up or obtained in another function, it is less expensive to create a variable in memory and quickly use access, this happens for example with: timings.endTimestamp, timings.vestingStartTimestamp and auctionParams.baseToken in createAuction(), also with bidIndices.length and seenBidMap.length in finalize().

- L244/302 - Instead of i++ it is less expensive to do unchecked{++i;}

- L313/319 - If previously it is validated that a subtraction operation is positive, when the operation is performed it can be unchecked.
