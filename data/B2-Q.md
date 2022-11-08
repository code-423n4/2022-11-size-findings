## Event is missing `indexed` fields

Each event should use three indexed fields if there are three or more fields.

```
    event AuctionCreated(
        uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
```
>File: src/interfaces/ISizeSealed.sol [(line 97-98)](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L97-L98)
```
    event Bid(
        address sender,
        uint256 auctionId,
        uint256 bidIndex,
        uint128 quoteAmount,
        bytes32 commitment,
        ECCMath.Point pubKey,
        bytes32 encryptedMessage,
        bytes encryptedPrivateKey
```
>File: src/interfaces/ISizeSealed.sol [(line 103-111)](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L103-L111)
```
       Other instances of this issue are:
```
* https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L118
* https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L122

## Use of `block.timestamp`

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers, locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
        if (block.timestamp < a.timings.startTimestamp) {
```
>File: src/SizeSealed.sol [(line 29)](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L29)
```
        } else if (block.timestamp < a.timings.endTimestamp) {
```
>File: src/SizeSealed.sol [(line 31)](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L31)
```
       Other instances of this issue are:
```
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L35
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L37
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L60
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L425
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L426
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L460

## TYPOS

```
    ///@audit: @`runnning`
    /// @notice Bid on a runnning auction
```
>File: src/SizeSealed.sol [(line 112)](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L112)
```
    ///@audit: @`futher`
    // Prevent any futher access to this EncryptedBid
```
>File: src/SizeSealed.sol [(line 431)](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L431)

## `revert()` should have descriptive reason strings 

```
       30            if (_state != States.Created) revert InvalidState();
      
       32            if (_state != States.AcceptingBids) revert InvalidState();
 
       34            if (_state != States.Finalized) revert InvalidState();

       36            if (_state != States.RevealPeriod) revert InvalidState();

       38            if (_state != States.Voided) revert InvalidState();

       40            revert();

```
>File: src/SizeSealed.sol (https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol)

All `revert()` in the file `src/SizeSealed.sol` lacks descriptive reason strings .

## Usage of `uints/ints` smaller than 32 bytes (256 bits) incurs overhead
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```
        uint128 quoteAmount,
```
>File: src/SizeSealed.sol [(line 124)](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L124)
```
        uint32 startTimestamp;
        uint32 endTimestamp;
        uint32 vestingStartTimestamp;
        uint32 vestingEndTimestamp;
```
>File: src/interfaces/ISizeSealed.sol[(line 56-59)](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L56-L59)
```
       Other instances of this issue are:
```
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L205-L206
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L365
* https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L451
* https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L48-L52
* https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L65-L67

## `NatSpec` is incomplete

```
     /// @audit Missing: '@param:auctionId`
     /// @notice Reveals the private key of the seller
     /// @dev All valid bids are decrypted after this
     ///      finalizeData should be empty if seller does not wish to finalize in this tx
     /// @param privateKey Private key corresponding to the auctions public key
     /// @param finalizeData Calldata that will be sent to finalize()
    function reveal(uint256 auctionId, uint256 privateKey, bytes calldata finalizeData)
```
* File: /src/SizeSealed.sol [(line 172-177)](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L172-L177)
```
     /// @audit Missing: '@return`
    /// @notice calculates point^scalar
    /// @dev returns (1,1) if the ecMul failed or invalid parameters
    /// @return corresponding point
    function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {
```
* File: src/util/ECCMath [(line 22-25)](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L22-L25)
```
       Other instances of this issue are:
```
* https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L33-L37        
///@audit Missing: `@retun`
* https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L48-L51        
///@audit Missing: `@retun`
* https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L16-L18        
///@audit Missing: `@retun`&`@param`
* https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L59-L60        
///@audit Missing: `@retun`&`@param`