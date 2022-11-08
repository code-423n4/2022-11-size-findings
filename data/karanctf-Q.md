# Low 

## 1. Use of `Block.timestamp`
**Summary:** Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
```solidity

SizeSealed.sol:29:        if (block.timestamp < a.timings.startTimestamp) {
SizeSealed.sol:31:        } else if (block.timestamp < a.timings.endTimestamp) {
SizeSealed.sol:35:        } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
SizeSealed.sol:37:        } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
SizeSealed.sol:60:        if (timings.endTimestamp <= block.timestamp) {
SizeSealed.sol:425:        if (block.timestamp >= a.timings.endTimestamp) {
SizeSealed.sol:426:            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {
SizeSealed.sol:460:            uint32(block.timestamp),
```


## 2. `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`
```solidity
SizeSealed.sol-129-        bytes32[] calldata proof
SizeSealed.sol-130-    ) external atState(idToAuction[auctionId], States.AcceptingBids) returns (uint256) {
SizeSealed.sol-131-        Auction storage a = idToAuction[auctionId];
SizeSealed.sol-132-        if (a.params.merkleRoot != bytes32(0)) {
SizeSealed.sol:133:            bytes32 leaf = keccak256(abi.encodePacked(msg.sender));
SizeSealed.sol-134-            if (!MerkleProofLib.verify(proof, a.params.merkleRoot, leaf)) {
SizeSealed.sol-135-                revert InvalidProof();
SizeSealed.sol-136-            }
SizeSealed.sol-137-        }
--
SizeSealed.sol-467-        return keccak256(abi.encode(message));
SizeSealed.sol-468-    }
SizeSealed.sol-469-
SizeSealed.sol-470-    function computeMessage(uint128 baseAmount, bytes16 salt) external pure returns (bytes32) {
SizeSealed.sol:471:        return bytes32(abi.encodePacked(baseAmount, salt));
SizeSealed.sol-472-    }
SizeSealed.sol-473-
SizeSealed.sol-474-    function getTimings(uint256 auctionId) external view returns (Timings memory timings) {
SizeSealed.sol-475-        timings = idToAuction[auctionId].timings;
```

# Non Critical
## 1. Use `allowlist/denylist` rather than `blacklist/whitelist`
```solidity
SizeSealed.sol:121:    /// @param proof Merkle proof that checks seller against `merkleRoot` if there is a whitelist
```

