- [Low](#low)
    - [**1. Bid balance not ensured**](#1-bid-balance-not-ensured)
    - [**2. Unfair game for buyers**](#2-unfair-game-for-buyers)
    - [**3. Design weakness**](#3-design-weakness)
    - [**4. Seller can lock buyer tokens**](#4-seller-can-lock-buyer-tokens)
- [Non critical](#non-critical)
    - [**5. Integer overflow by unsafe casting**](#5-integer-overflow-by-unsafe-casting)
    - [**6. Add indexes to events**](#6-add-indexes-to-events)
    - [**7. Add missing @param information**](#7-add-missing-param-information)
    - [**8. Be specific with comments**](#8-be-specific-with-comments)
    - [**9. Unify return criteria**](#9-unify-return-criteria)
    - [**10. Don't worry about keccak256 collisions**](#10-dont-worry-about-keccak256-collisions)

# Low

## **1. Bid balance not ensured**

During `createAuction` the received `baseToken` balance is ensured to be received exactly, in order to avoid possible fees. 

```js
uint256 balanceBeforeTransfer = ERC20(auctionParams.baseToken).balanceOf(address(this));

SafeTransferLib.safeTransferFrom(
    ERC20(auctionParams.baseToken), msg.sender, address(this), auctionParams.totalBaseAmount
);

uint256 balanceAfterTransfer = ERC20(auctionParams.baseToken).balanceOf(address(this));
if (balanceAfterTransfer - balanceBeforeTransfer != auctionParams.totalBaseAmount) {
    revert UnexpectedBalanceChange();
}
```

This check is not present during the `bid` procedure, is trusted in the `quoteToken`, but the true is because there are no token white list, the same checks should be done in order to ensure to be full compatible with tokens with fee (or empty).

```js
SafeTransferLib.safeTransferFrom(ERC20(a.params.quoteToken), msg.sender, address(this), quoteAmount);
```

**So `quoteToken` will allow empty address and it won't pass https://github.com/transmissions11/solmate/blob/main/src/utils/SafeTransferLib.sol#L9.** 

**Affected source code:**

- [SizeSealed.sol:163](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L163)

## **2. Unfair game for buyers**

`cancelAuction` is available until the seller call `reveal` because only after that moment it's possible to call `finalize`.

The problem with this is that the buyers have committed themselves by spending gas to place a bid, and once the seller has had all the bids on the table, he can decide to leave, without selling, without disclosing and knowing all the bids, which It is like doing a market analysis without providing information, at the expense of the buyers, something that is not very advantageous for the buyer.

For that reason, I think that it should **not be possible** to cancel an auction if it's **under `RevealPeriod`**, and the seller should pay the gas cost of returning all bids to buyers upon cancellation.

**Affected source code:**

- [SizeSealed.sol:398](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L398)

## **3. Design weakness**

As the `SizeSealed` contract works, the buyer has to provide his bid in an encrypted way, however during the creation of the bid he has to provide the collateral for it. This makes it easier for buyers to adjust their collateral to their bid, so that the fact of encrypting it is meaningless, it would be more convenient for buyers to have a general balance, not related to the bid, and that the same balance be made on this balance, in this way, the balance of the sent user would not be related to the bid in question, and since someone is not incentivized in any way to send 100 eth and bid for 1, it would be more beneficial for buyers when it comes to do not reveal your move.

**Affected source code:**

- [SizeSealed.sol](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol)

## **4. Seller can lock buyer tokens**

The seller specify the vesting time for buyer's baseTokens, this value is not controlled and it's used for `tokensAvailableForWithdrawal` in order to allow buyers to `withdraw` his tokens. The problem is if the seller set an incredible high value, these tokens will be locked forever and buyer will loss his tokens for all of his life, this information is public, but maybe the date format is shown short in the dApp, like 1/10/23, and the year it's 3023, so it could be easy for the users to bid for a hole.

**Affected source code:**

- [SizeSealed.sol:66-70](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L66-L70)

---

# Non critical

## **5. Integer overflow by unsafe casting**

Keep in mind that the version of solidity used, despite being greater than `0.8`, does not prevent integer overflows during casting, it only does so in mathematical operations.

It is necessary to safely convert between the different numeric types.

**Recommendation:**

Use a [safeCast](https://docs.openzeppelin.com/contracts/3.x/api/utils#SafeCast) from Open Zeppelin or increase the type length.

```javascript
        uint32(block.timestamp)
```

*In this case the date will overflow in: Sun Feb 07 2106 (2^32 - 1 equals to 4294967295)*

**Affected source code:**

- [SizeSealed.sol:460](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L460)
- [CommonTokenMath.sol:62-66](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/CommonTokenMath.sol#L62-L66)

## **6. Add indexes to events**

Solidity indexes help dApps and users to filter and process the information provided by smart contracts. That is why adding indexes in values that may be interesting for the user improves the usability of the contract.

```diff
    event AuctionCreated(
-       uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
+       uint256 indexed auctionId, address indexed seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
    );

    event AuctionCancelled(uint256 auctionId);

    event Bid(
-       address sender,
-       uint256 auctionId,
+       address indexed sender,
+       uint256 indexed auctionId,
        uint256 bidIndex,
        uint128 quoteAmount,
        bytes32 commitment,
        ECCMath.Point pubKey,
        bytes32 encryptedMessage,
        bytes encryptedPrivateKey
    );

-   event BidCancelled(uint256 auctionId, uint256 bidIndex);
+   event BidCancelled(uint256 indexed auctionId, uint256 bidIndex);

-   event RevealedKey(uint256 auctionId, uint256 privateKey);
+   event RevealedKey(uint256 indexed auctionId, uint256 privateKey);

-   event AuctionFinalized(uint256 auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);
+   event AuctionFinalized(uint256 indexed auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);

-   event BidRefund(uint256 auctionId, uint256 bidIndex);
+   event BidRefund(uint256 indexed auctionId, uint256 bidIndex);

-   event Withdrawal(uint256 auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);
+   event Withdrawal(uint256 indexed auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);
```

**Reference:**

- https://docs.soliditylang.org/en/latest/contracts.html#events

**Affected source code:**

- [ISizeSealed.sol:97-122](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L97-L122)

## **7. Add missing @param information**

Some parameters have not been commented, which reflects that the logic has been updated but not the documentation, it is advisable that developers update both at the same time to avoid the code being out of sync with the project documentation.

**Affected source code:**

- In [ISizeSealed.sol:82](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L82):

```diff
    /// @param baseToken The ERC20 to be sold by the seller
    /// @param quoteToken The ERC20 to be bid by the bidders
    /// @param reserveQuotePerBase Minimum price that bids will be filled at
    /// @param totalBaseAmount Max amount of `baseToken` to be auctioned
    /// @param minimumBidQuote Minimum quote amount a bid can buy
+   /// @param merkleRoot seller's merkleRoot for whitelist bidders
    /// @param pubKey On-chain storage of seller's ephemeral public key
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

- In [SizeSealed.sol:177](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L177):

```diff
    /// @notice Reveals the private key of the seller
    /// @dev All valid bids are decrypted after this
    ///      finalizeData should be empty if seller does not wish to finalize in this tx
+   /// @param auctionId `auctionId` of the auction to bid on
    /// @param privateKey Private key corresponding to the auctions public key
    /// @param finalizeData Calldata that will be sent to finalize()
    function reveal(uint256 auctionId, uint256 privateKey, bytes calldata finalizeData)
        external
        atState(idToAuction[auctionId], States.RevealPeriod)
    {
```

- In [ECCMath.sol:51](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L51):

```diff
    /// @notice decrypts a message that was encrypted using `encryptMessage()`
    /// @param sharedPoint G^k1^k2 where k1 and k2 are the
+   /// @param encryptedMessage encrypted message proporcionated by the other part
    ///      private keys of the two parties that can decrypt
    function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)
        internal
        pure
```

## **8. Be specific with comments**

`cliffPercent` is based on 1e18, but this information was not shown in the code documentation.

```diff
    /// @dev Helper function to determine tokens at a specific `block.timestamp`
    /// @return tokensAvailable Amount of unlocked `baseToken` at the current `block.timestamp`
    /// @param vestingStart Start of linear vesting
    /// @param vestingEnd Completion of linear vesting
    /// @param currentTime Timestamp to evaluate at
-   /// @param cliffPercent Normalized percent to unlock at vesting start
+   /// @param cliffPercent Normalized percent to unlock at vesting start based on 1e18 factor
    /// @param baseAmount Total amount of vested `baseToken`
    function tokensAvailableAtTime(
        uint32 vestingStart,
        uint32 vestingEnd,
        uint32 currentTime,
        uint128 cliffPercent,
        uint128 baseAmount
    ) internal pure returns (uint128) {
        if (currentTime > vestingEnd) {
            return baseAmount; // If vesting is over, bidder is owed all tokens
        } else if (currentTime <= vestingStart) {
            return 0; // If cliff hasn't been triggered yet, bidder receives no tokens
        } else {
            // Vesting is active and cliff has triggered
            uint256 cliffAmount = FixedPointMathLib.mulDivDown(baseAmount, cliffPercent, 1e18);

            return uint128(
                cliffAmount
                    + FixedPointMathLib.mulDivDown(
                        baseAmount - cliffAmount, currentTime - vestingStart, vestingEnd - vestingStart
                    )
            );
        }
    }
```

**Affected source code:**

- [CommonTokenMath.sol:45](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/CommonTokenMath.sol#L45)

## **9. Unify return criteria**

In `ECCMath.sol` and `SizeSealed.sol` files, the methods are sometimes returned with return, other times the return variable is equal, and on other occasions the name of the return variable is not defined, unifying the way of writing the code makes the code more uniform and readable.

```diff
+   function publicKey(uint256 privateKey) internal view returns (Point memory) {
+       return ecMul(Point(GX, GY), privateKey);
    }

+   function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {
        bytes memory data = abi.encode(point, scalar);
        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);
        (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(data);
        if (!res) return Point(1, 1);
+       return abi.decode(ret, (Point));
    }

+   function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)
        internal view
        returns (Point memory buyerPub, bytes32 encryptedMessage)
    {
        Point memory sharedPoint = ecMul(encryptToPub, encryptWithPriv);
        bytes32 sharedKey = hashPoint(sharedPoint);
+       encryptedMessage = message ^ sharedKey;
+       buyerPub = publicKey(encryptWithPriv);
    }

    function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)
        internal pure
+       returns (bytes32 decryptedMessage)
    {
+       return encryptedMessage ^ hashPoint(sharedPoint);
    }

+   function hashPoint(Point memory point) internal pure returns (bytes32) {
+       return keccak256(abi.encode(point));
    }
```

**Affected source code:**

- [ECCMath.sol:18-60](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L18-L60)
- [SizeSealed.sol:466-480](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L466-L480)

## **10. Don't worry about keccak256 collisions**

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

In case of the result of `computeCommitment` returns 0, because a collision or an error, a cancelled bid will be processed as valid.

```js
            bytes32 decryptedMessage = ECCMath.decryptMessage(sharedPoint, b.encryptedMessage);
            // If the bidder didn't faithfully submit commitment or pubkey
            // Or the bid was cancelled
            if (computeCommitment(decryptedMessage) != b.commitment) continue;
```

It's more resilient to use a different value as flag, `b.pubKey` will be safer and cheaper.

**Affected source code:**

- [SizeSealed.sol:258](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L258)
- [SizeSealed.sol:435](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L435)
