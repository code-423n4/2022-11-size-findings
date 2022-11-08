## Unreasonably Complex Code

The following code is very hard to read and may or may not contain subtle errors.
Consider rewriting, maybe something along the lines of:

    if(_state== ABC){
        if(..) revert();
        return;
    }
    if(_state== DEF){
        if(..) revert();
        return;
    }

Instead of:

    if (block.timestamp < a.timings.startTimestamp) {
        if (_state != States.Created) revert InvalidState();
    } else if (block.timestamp < a.timings.endTimestamp) {
        if (_state != States.AcceptingBids) revert InvalidState();
    } else if (a.data.lowestQuote != type(uint128).max) {
        if (_state != States.Finalized) revert InvalidState();
    } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
        if (_state != States.RevealPeriod) revert InvalidState();
    } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
        if (_state != States.Voided) revert InvalidState();
    } else {
        revert();
    }
    _;
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L29-L42

## Use Constant

'24 hours' is used in at least three places. This should be a constant.

    } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
        if (_state != States.RevealPeriod) revert InvalidState();
    } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L35-L37

    if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L426

## Sensitive Terms

Avoid the use of sensitive terms in favor of neutral ones. Use allowlist rather than whitelist.

    /// @param proof Merkle proof that checks seller against `merkleRoot` if there is a whitelist
https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol#L121

## Missing Indexed Fields in Events

Each event should use three indexed fields if there are three or more fields in the event.

***

event AuctionCreated(
    uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
);
https://github.com/code-423n4/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L97-L99

event Bid(
    address sender,
    uint256 auctionId,
    uint256 bidIndex,
    uint128 quoteAmount,
    bytes32 commitment,
    ECCMath.Point pubKey,
    bytes32 encryptedMessage,
    bytes encryptedPrivateKey
);
https://github.com/code-423n4/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L103-L112

event AuctionFinalized(uint256 auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);
https://github.com/code-423n4/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L118

event Withdrawal(uint256 auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);
https://github.com/code-423n4/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L122

## Revert and Require Should Have Descriptive Reason Strings

revert();
https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol#L40