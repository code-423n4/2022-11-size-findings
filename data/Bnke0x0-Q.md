

### [N01] Adding a return statement when the function defines a named return variable, is redundant


#### Findings:
```
2022-11-size/src/SizeSealed.sol::109 => return auctionId;
2022-11-size/src/SizeSealed.sol::169 => return bidIndex;
2022-11-size/src/util/CommonTokenMath.sol::55 => return baseAmount; // If vesting is over, bidder is owed all tokens
```



### [N02] `require()`/`revert()` statements should have descriptive reason strings


#### Findings:
```
2022-11-size/src/SizeSealed.sol::40 => revert();
```



### [N03] Event is missing `indexed` fields

#### Impact
Each event should use three indexed fields if there are three or more fields
#### Findings:
```
2022-11-size/src/interfaces/ISizeSealed.sol::97 => event AuctionCreated(
2022-11-size/src/interfaces/ISizeSealed.sol::103 => event Bid(
2022-11-size/src/interfaces/ISizeSealed.sol::114 => event BidCancelled(uint256 auctionId, uint256 bidIndex);
2022-11-size/src/interfaces/ISizeSealed.sol::116 => event RevealedKey(uint256 auctionId, uint256 privateKey);
2022-11-size/src/interfaces/ISizeSealed.sol::118 => event AuctionFinalized(uint256 auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);
2022-11-size/src/interfaces/ISizeSealed.sol::120 => event BidRefund(uint256 auctionId, uint256 bidIndex);
2022-11-size/src/interfaces/ISizeSealed.sol::122 => event Withdrawal(uint256 auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);
```

### [N04] States/flags should use Enums rather than separate constants


#### Findings:
```
2022-11-size/src/util/ECCMath.sol::8 => uint256 internal constant GX = 1;
2022-11-size/src/util/ECCMath.sol::9 => uint256 internal constant GY = 2;
```



### [N05] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from public to external .
#### Findings:
```
2022-11-size/src/SizeSealed.sol::466 => function computeCommitment(bytes32 message) public pure returns (bytes32) {
```



### [N06] Numeric values having to do with time should use time units for readability

#### Impact
There are units for seconds, minutes, hours, days, and weeks
#### Findings:
```
2022-11-size/src/SizeSealed.sol::35 => } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
2022-11-size/src/SizeSealed.sol::37 => } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
2022-11-size/src/SizeSealed.sol::426 => if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {
```



