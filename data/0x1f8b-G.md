- [Gas](#gas)
    - [**1. Optimize finalize with cancelled bids**](#1-optimize-finalize-with-cancelled-bids)
    - [**2. Avoid storage keyword during modifiers**](#2-avoid-storage-keyword-during-modifiers)
    - [**3. Reorder structure layout**](#3-reorder-structure-layout)
    - [**4. Avoid compound assignment operator in state variables**](#4-avoid-compound-assignment-operator-in-state-variables)
        - [Total gas saved: **13 * 2 = 26**](#total-gas-saved-13--2--26)
    - [**5. Optimize variable definitions**](#5-optimize-variable-definitions)
    - [**6. Optimize finalize conditions**](#6-optimize-finalize-conditions)

# Gas

## **1. Optimize `finalize` with cancelled bids**

When `cancelBid` is called, the `commitment` is set to 0 in order to force to fail the `computeCommitment` comparison.

```js
    function cancelBid(uint256 auctionId, uint256 bidIndex)
        external
    {
        ...

        // Prevent any futher access to this EncryptedBid
        b.sender = address(0);

        // Prevent seller from finalizing a cancelled bid
        b.commitment = 0;
```

 This can be cheaper if `b.pubKey` is set to 0,0, because `ECCMath.ecMul` will return a (1,1) point and it will save all the `decryptMessage` from cancelled bids.

```js
            ECCMath.Point memory sharedPoint = ECCMath.ecMul(b.pubKey, sellerPriv);
            // If the bidder public key isn't on the bn128 curve
            if (sharedPoint.x == 1 && sharedPoint.y == 1) continue;

            bytes32 decryptedMessage = ECCMath.decryptMessage(sharedPoint, b.encryptedMessage);
            // If the bidder didn't faithfully submit commitment or pubkey
            // Or the bid was cancelled
            if (computeCommitment(decryptedMessage) != b.commitment) continue;
```

*Note: This also will be more secure because `computeCommitment` could return 0 with a collision, and process the cancelled bid as valid.*

**Affected source code:**

- [SizeSealed.sol:258](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L258)
- [SizeSealed.sol:435](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L435)

## **2. Avoid `storage` keyword during modifiers**

The problem with using modifiers with storage variables is that these accesses will likely have to be obtained multiple times, to pass it to the modifier, and to read it into the method code.

```js
    modifier atState(Auction storage a, States _state) {
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
    }
```

As you can see the following methods require to extract the `Auction` twice instead of once, it is recommended to use a method instead of a modifier

```js
    function bid(...) external 
        atState(idToAuction[auctionId], States.AcceptingBids) 
        returns (uint256) {
            Auction storage a = idToAuction[auctionId];
```

**Affected source code:**

- [SizeSealed.sol:130-131](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L130-L131)
- [SizeSealed.sol:179-181](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L179-L181)
- [SizeSealed.sol:219-221](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L219-L221)
- [SizeSealed.sol:336-337](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L336-L337)
- [SizeSealed.sol:359](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L359)

## **3. Reorder structure layout**

The following structures could be optimized moving the position of certain values in order to save a lot slots:

```diff
    struct EncryptedBid {
        address sender;
+       ECCMath.Point pubKey;
        uint128 quoteAmount;
        uint128 filledBaseAmount;
        uint128 baseWithdrawn;
        bytes32 commitment;
-       ECCMath.Point pubKey;
        bytes32 encryptedMessage;
    }
```

```diff
    struct AuctionParameters {
        address baseToken;
        address quoteToken;
+       ECCMath.Point pubKey;
        uint256 reserveQuotePerBase;
        uint128 totalBaseAmount;
        uint128 minimumBidQuote;
        bytes32 merkleRoot;
-       ECCMath.Point pubKey;
    }
```

**Reference:**

- https://ethdebug.github.io/solidity-data-representation

> Enums are represented by integers; the possibility listed first by 0, the next by 1, and so forth. An enum type just acts like uintN, where N is the smallest legal value large enough to accomodate all the possibilities.

- https://docs.soliditylang.org/en/v0.8.17/internals/layout_in_storage.html#storage-inplace-encoding

**Affected source code:**

- [ISizeSealed.sol:46](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L46)
- [ISizeSealed.sol:83](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L83)

## **4. Avoid compound assignment operator in state variables**

Using compound assignment operators for state variables (like `State += X` or `State -= X` ...) it's more expensive than using operator assignment (like `State = State + X` or `State = State - X` ...).

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
uint256 private _a;
function testShort() public {
_a += 1;
}
}

contract TesterB {
Uint256 private _a;
function testLong() public {
_a = _a + 1;
}
}
```

Gas saving executing: **13 per entry**

```
TesterA.testShort: 43507
TesterB.testLong:  43494
```

**Affected source code:**

- [SizeSealed.sol:294](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L294)
- [SizeSealed.sol:373](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L373)

### Total gas saved: **13 * 2 = 26**

## **5. Optimize variable definitions**

Where a variable is defined, it is important to avoid unnecessary expense prior to the appropriate validations.

**Affected source code:**

- [ECCMath.sol:26](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L26) can be optimized like:

```diff
    /// @notice calculates point^scalar
    /// @dev returns (1,1) if the ecMul failed or invalid parameters
    /// @return corresponding point
    function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {
-       bytes memory data = abi.encode(point, scalar);
        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);
-       (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(data);
+       (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(abi.encode(point, scalar));
        if (!res) return Point(1, 1);
        return abi.decode(ret, (Point));
    }
```

- [SizeSealed.sol:157](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L157) can be optimized like:

```diff
    function bid(
        uint256 auctionId,
        uint128 quoteAmount,
        bytes32 commitment,
        ECCMath.Point calldata pubKey,
        bytes32 encryptedMessage,
        bytes calldata encryptedPrivateKey,
        bytes32[] calldata proof
    ) external atState(idToAuction[auctionId], States.AcceptingBids) returns (uint256) {
        ...

+       uint256 bidIndex = a.bids.length;
+       // Max of 1000 bids on an auction to prevent DOS
+       if (bidIndex >= 1000) {
+           revert InvalidState();
+       }

        EncryptedBid memory ebid;
        ebid.sender = msg.sender;
        ebid.quoteAmount = quoteAmount;
        ebid.commitment = commitment;
        ebid.pubKey = pubKey;
        ebid.encryptedMessage = encryptedMessage;

-       uint256 bidIndex = a.bids.length;
-       // Max of 1000 bids on an auction to prevent DOS
-       if (bidIndex >= 1000) {
-           revert InvalidState();
-       }

        a.bids.push(ebid);

        SafeTransferLib.safeTransferFrom(ERC20(a.params.quoteToken), msg.sender, address(this), quoteAmount);

        emit Bid(
            msg.sender, auctionId, bidIndex, quoteAmount, commitment, pubKey, encryptedMessage, encryptedPrivateKey
        );

        return bidIndex;
    }
```

## **6. Optimize `finalize` conditions**

If the auction has been fully filled, it's not required to decrypt the message and do nothing else.

```diff
    function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
        public
        atState(idToAuction[auctionId], States.RevealPeriod)
    {
        ...

        // Fill orders from highest price to lowest price
        for (uint256 i; i < bidIndices.length; i++) {
            uint256 bidIndex = bidIndices[i];
            EncryptedBid storage b = a.bids[bidIndex];

            // Verify this bid index hasn't been seen before
            uint256 bitmapIndex = bidIndex / 256;
            uint256 bitMap = seenBidMap[bitmapIndex];
            uint256 indexBit = 1 << (bidIndex % 256);
            if (bitMap & indexBit == 1) revert InvalidState();
            seenBidMap[bitmapIndex] = bitMap | indexBit;

+           // Auction has been fully filled
+           if (data.filledBase == data.totalBaseAmount) continue;

            // G^k1^k2 == G^k2^k1
            ECCMath.Point memory sharedPoint = ECCMath.ecMul(b.pubKey, sellerPriv);
            // If the bidder public key isn't on the bn128 curve
            if (sharedPoint.x == 1 && sharedPoint.y == 1) continue;

            bytes32 decryptedMessage = ECCMath.decryptMessage(sharedPoint, b.encryptedMessage);
            // If the bidder didn't faithfully submit commitment or pubkey
            // Or the bid was cancelled
            if (computeCommitment(decryptedMessage) != b.commitment) continue;

            // First 128 bits are the base amount, last are random salt
            uint128 baseAmount = uint128(uint256(decryptedMessage >> 128));

            // Require that bids are passed in descending price
            uint256 quotePerBase = FixedPointMathLib.mulDivDown(b.quoteAmount, type(uint128).max, baseAmount);
            if (quotePerBase >= data.previousQuotePerBase) {
                // If last bid was the same price, make sure we filled the earliest bid first
                if (quotePerBase == data.previousQuotePerBase) {
                    if (data.previousIndex > bidIndex) revert InvalidSorting();
                } else {
                    revert InvalidSorting();
                }
            }

            // Only fill if above reserve price
            if (quotePerBase < data.reserveQuotePerBase) continue;

-           // Auction has been fully filled
-           if (data.filledBase == data.totalBaseAmount) continue;

            ...
        }

        ...
    }
```

**Affected source code:**

- [SizeSealed.sol:283](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L283)
