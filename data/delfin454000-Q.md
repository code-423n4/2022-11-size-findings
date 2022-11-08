### Named return variables are not used when a function returns
___
Named return variable (here, `tokensAvailable`) is never used

[SizeSealed.sol: L451-454](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L451-L454)
```solidity
    function tokensAvailableForWithdrawal(uint256 auctionId, uint128 baseAmount)
        public
        view
        returns (uint128 tokensAvailable)
```
___
___


### `Event` is missing `indexed` fields
Each `event` should use three `indexed` fields if there are three or more fields

___
[ISizeSealed.sol: L97-99](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L97-L99)
```solidity
    event AuctionCreated(
        uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
    );
```
___

Similarly for the following `events`:

[ISizeSealed.sol: L103-112](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L103-L112)

[ISizeSealed.sol: L118](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L118)

[ISizeSealed.sol: L122](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L122)
___
___


### Missing `@param` statements

[SizeSealed.sol: L172-178](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L172-L178)
```solidity
    /// @notice Reveals the private key of the seller
    /// @dev All valid bids are decrypted after this
    ///      finalizeData should be empty if seller does not wish to finalize in this tx
    /// @param privateKey Private key corresponding to the auctions public key
    /// @param finalizeData Calldata that will be sent to finalize()
    function reveal(uint256 auctionId, uint256 privateKey, bytes calldata finalizeData)
        external
```
Missing: `@param auctionId`
___
[ISizeSealed.sol: L70-84](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L70-L84)
```solidity
    /// @param baseToken The ERC20 to be sold by the seller
    /// @param quoteToken The ERC20 to be bid by the bidders
    /// @param reserveQuotePerBase Minimum price that bids will be filled at
    /// @param totalBaseAmount Max amount of `baseToken` to be auctioned
    /// @param minimumBidQuote Minimum quote amount a bid can buy
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
Missing: `@param merkleRoot`
___

### Missing NatSpec

NatSpec is completely missing for the following `function`:

[SizeSealed.sol: L466-467](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L466-L467)
```solidity
    function computeCommitment(bytes32 message) public pure returns (bytes32) {
        return keccak256(abi.encode(message));
```
Similarly for the following `public` or `external` `functions`:

[SizeSealed.sol: L470-471](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L470-L471)

[SizeSealed.sol: L474-475](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L474-L475)

[SizeSealed.sol: L478-479](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L478-L479)
___

NatSpec is also completely missing for the following `struct`:

[SizeSealed.sol: L202-209](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L202-L209)
```solidity
    // Used to get around stack too deep errors -- even with viaIr
    struct FinalizeData {
        uint256 reserveQuotePerBase;
        uint128 totalBaseAmount;
        uint128 filledBase;
        uint256 previousQuotePerBase;
        uint256 previousIndex;
    }
```
Similarly for the following `structs`:

[ISizeSealed.sol: L40-48](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L40-L48)

[ISizeSealed.sol: L63-68](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L63-L68)

[ISizeSealed.sol: L86-91](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L86-L91)
___
___


### Typos
___

[SizeSealed.sol: L112](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L112)
```solidity
    /// @notice Bid on a runnning auction
```
Change `runnning` to `running`
___
[SizeSealed.sol: L211](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L211)
```solidity
    /// @notice Finalises an auction by revealing all bids
```
Change `Finalises` to `Finalizes`. Note that the American English version of this word ("finalize") and its variants are used throughout for vaiable names and most of the remainder of comments in the contest.
___
[SizeSealed.sol: L412](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L412)
```solidity
    /// @dev Transfers `quoteToken` back to bidder and prevents bid from being finalised
```
Change `finalised` to `finalized`
___
[SizeSealed.sol: L431](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L431)
```solidity
        // Prevent any futher access to this EncryptedBid
```
Change `futher` to `futher`
___
___


### Open item
Comments that refer to open items should be addressed
 
[ECCMath.sol: L59](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/util/ECCMath.sol#L59)
```solidity
    /// @dev we hash the point because unsure if x,y is normal distribution (source needed)
```
___
___


### Update sensitive terms
Terms incorporating "black," "white," "slave" or "master" are potentially problematic. Substituting more neutral terminology is becoming [common practice](https://www.zdnet.com/article/mysql-drops-master-slave-and-blacklist-whitelist-terminology/).
___
[SizeSealed.sol: L121](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L121)
```solidity
    /// @param proof Merkle proof that checks seller against `merkleRoot` if there is a whitelist
```
Suggestion: Change `whitelist` to `allowlist` 
___
___
