# Base token can be set equal to quote token when creating auction

When creating a auction, 

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L55

the base token and quote token can be the same token (same address).

To avoid such misconfiguration, 

We recommend the project add the check

```solidity
if(auctionParams.baseToken == auctionParams.quoteToken) revert InvalidAuctionTokenSetting();
```

# startTimestamp can be set to past when creating auction

When creating auction, the code has this sanity check

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L55

```solidity
if (timings.endTimestamp <= block.timestamp) {
	revert InvalidTimestamp();
}
if (timings.startTimestamp >= timings.endTimestamp) {
	revert InvalidTimestamp();
}
if (timings.endTimestamp > timings.vestingStartTimestamp) {
	revert InvalidTimestamp();
}
if (timings.vestingStartTimestamp > timings.vestingEndTimestamp) {
	revert InvalidTimestamp();
}
```

this make sure 

start timestamp < end timstamp < start vesting timestamp < end vesting timestamp.

and current timestamp < end timestamp,

However, the code does not check if

current timstamp < start timestamp,

the seller can set the start timestamp to the past. The impact is low, but it just does not make sense,

We recommend the project add the check

```solidity
if (timings.startTimestamp < block.timestamp) {
	revert InvalidTimestamp();
}
```

# the code stop working at uint32(block.timestamp)

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L460

the will not be working after the block.timestamp pass the timestamp type(uint32).max

```solidity
function tokensAvailableForWithdrawal(uint256 auctionId, uint128 baseAmount)
	public
	view
	returns (uint128 tokensAvailable)
{
	Auction storage a = idToAuction[auctionId];
	return CommonTokenMath.tokensAvailableAtTime(
		a.timings.vestingStartTimestamp,
		a.timings.vestingEndTimestamp,
		uint32(block.timestamp),
		a.timings.cliffPercent,
		baseAmount
	);
}
```

We recommend the project just uint256 for the timestamp.

# token amount larger than uint256 is not supported.

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L40

```
struct EncryptedBid {
	address sender;
	uint128 quoteAmount;
	uint128 filledBaseAmount;
	uint128 baseWithdrawn;
	bytes32 commitment;
	ECCMath.Point pubKey;
	bytes32 encryptedMessage;
}
```

The code use uint128 for token amount extensively.

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L76

```solidity
struct AuctionParameters {
	address baseToken;
	address quoteToken;
	uint256 reserveQuotePerBase;
	uint128 totalBaseAmount;
	uint128 minimumBidQuote;
	bytes32 merkleRoot;
	ECCMath.Point pubKey;
}
```

this means that the code will not support the auction if the token amount larger than type(uint128).max.

We recommend the project just use uint256 to handle the token amount.