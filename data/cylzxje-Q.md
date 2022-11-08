### [L-01]  More inclusive check on `timings.vesting`
src/SizeSealed.sol
- https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L69
- https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L79

Recommend changing to `>=` because  timings.vestingStartTimestamp != timings.vestingEndTimestamp
                                                                      minimumBidQuote != reserveQuotePerBase

### [N-01]  Event should be indexed
src/interfaces/ISizeSealed.sol
- https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L97-L122

Recommend adding `indexed` for address sender, uint256 auctionId to deal with off-chain monitoring

