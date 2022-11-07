## [L-01] Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
2022-11-size/src/SizeSealed.sol::29 => if (block.timestamp < a.timings.startTimestamp) {
2022-11-size/src/SizeSealed.sol::31 => } else if (block.timestamp < a.timings.endTimestamp) {
2022-11-size/src/SizeSealed.sol::35 => } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
2022-11-size/src/SizeSealed.sol::37 => } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
2022-11-size/src/SizeSealed.sol::60 => if (timings.endTimestamp <= block.timestamp) {
2022-11-size/src/SizeSealed.sol::425 => if (block.timestamp >= a.timings.endTimestamp) {
2022-11-size/src/SizeSealed.sol::426 => if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {
2022-11-size/src/SizeSealed.sol::460 => uint32(block.timestamp),
```

## [L-02] abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). Unless there is a compelling reason, abi.encode should be preferred. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

```
2022-11-size/src/SizeSealed.sol::133 => bytes32 leaf = keccak256(abi.encodePacked(msg.sender));
```

## [N-01] require()/revert() statements should have descriptive reason strings

Descriptive reason strings should be used so that users can troubleshot any reverted calls

```
2022-11-size/src/SizeSealed.sol::40 => revert();
```

## [N-02] Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

```
2022-11-size/src/interfaces/ISizeSealed.sol::101 => event AuctionCancelled(uint256 auctionId);
2022-11-size/src/interfaces/ISizeSealed.sol::114 => event BidCancelled(uint256 auctionId, uint256 bidIndex);
2022-11-size/src/interfaces/ISizeSealed.sol::116 => event RevealedKey(uint256 auctionId, uint256 privateKey);
2022-11-size/src/interfaces/ISizeSealed.sol::118 => event AuctionFinalized(uint256 auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);
2022-11-size/src/interfaces/ISizeSealed.sol::120 => event BidRefund(uint256 auctionId, uint256 bidIndex);
2022-11-size/src/interfaces/ISizeSealed.sol::122 => event Withdrawal(uint256 auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);
```

## [N-04] Adding a return statement when the function defines a named return variable, is redundant

It is not necessary to have both a named return and a return statement.

```
2022-11-size/src/util/ECCMath.sol::54 => returns (bytes32 decryptedMessage)
```
