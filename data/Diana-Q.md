## N-01 EVENT IS MISSING INDEXED FIELDS

Each `event` should use three `indexed` fields if there are three or more fields

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol

```
File: src/interfaces/ISizeSealed.sol

103-112: event Bid(
        address sender,
        uint256 auctionId,
        uint256 bidIndex,
        uint128 quoteAmount,
        bytes32 commitment,
        ECCMath.Point pubKey,
        bytes32 encryptedMessage,
        bytes encryptedPrivateKey
    );
```

------------------

## N-02 USE OF block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

_There are 8 instances of this issue_

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

```
File: src/SizeSealed.sol

29: if (block.timestamp < a.timings.startTimestamp) {
31: } else if (block.timestamp < a.timings.endTimestamp) {
35: } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
37: } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
60: if (timings.endTimestamp <= block.timestamp) {
425: if (block.timestamp >= a.timings.endTimestamp) {
426: if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {
460: uint32(block.timestamp),
```

### Recommended Mitigation Steps

Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

-------
