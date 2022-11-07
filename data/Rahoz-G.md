##  Add unchecked {} for subtractions where the operands cannot underflow
Because these calculation can't underflow so we can use unchecked to save gas
### Proof of Concept
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L378
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L371

## Using calldata instead of memory for read-only arguments in external functions saves gas
When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having memory arguments, it’s still valid for implementation contracs to use calldata arguments instead.
If the array is passed to an internal function which passes the array to another internal function where the array is modified and therefore memory is used in the external call, it’s still more gass-efficient to use calldata when the external function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one
Note that I’ve also flagged instances where the function is public but can be marked as external since it’s not called by the contract, and cases where a constructor is involved
`bidIndices`
### Proof of Concept
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L217
## Recommended Mitigation Steps
Replace memory with calldata

## STRUCTS CAN BE PACKED INTO FEWER STORAGE SLOTS
Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. 
Subsequent reads as well as writes have smaller gas savings
### Proof of Concept
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L76-L84

## CAN MOVE REVERT BEFORE CREATE EBID VARIABLE
Because we check `bidIndex` which query `Auction` length, if it greater than 1000 then revert, 
so we dont need to declare ebid variable before revert check
### Proof of Concept
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L148-L159
### Recommended Mitigation Steps
```solidity
function bid(
    uint256 auctionId,
    uint128 quoteAmount,
    bytes32 commitment,
    ECCMath.Point calldata pubKey,
    bytes32 encryptedMessage,
    bytes calldata encryptedPrivateKey,
    bytes32[] calldata proof
) external atState(idToAuction[auctionId], States.AcceptingBids) returns (uint256) {
    Auction storage a = idToAuction[auctionId];
    if (a.params.merkleRoot != bytes32(0)) {
        bytes32 leaf = keccak256(abi.encodePacked(msg.sender));
        if (!MerkleProofLib.verify(proof, a.params.merkleRoot, leaf)) {
            revert InvalidProof();
        }
    }

    // Seller cannot bid on their own auction
    if (msg.sender == a.data.seller) {
        revert UnauthorizedCaller();
    }

    if (quoteAmount == 0 || quoteAmount == type(uint128).max || quoteAmount < a.params.minimumBidQuote) {
        revert InvalidBidAmount();
    }

    uint256 bidIndex = a.bids.length;
    // Max of 1000 bids on an auction to prevent DOS
    if (bidIndex >= 1000) {
        revert InvalidState();
    }

    EncryptedBid memory ebid;
    ebid.sender = msg.sender;
    ebid.quoteAmount = quoteAmount;
    ebid.commitment = commitment;
    ebid.pubKey = pubKey;
    ebid.encryptedMessage = encryptedMessage;

    a.bids.push(ebid);

    SafeTransferLib.safeTransferFrom(ERC20(a.params.quoteToken), msg.sender, address(this), quoteAmount);

    emit Bid(
        msg.sender, auctionId, bidIndex, quoteAmount, commitment, pubKey, encryptedMessage, encryptedPrivateKey
    );

    return bidIndex;
}
```
