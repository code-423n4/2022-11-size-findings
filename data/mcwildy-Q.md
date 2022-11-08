# Qa Report

## SIZE

### Use `external` visibility modifier for function that are not being invoked by the contract

[SizeSealed.sol](https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol):

```solidity
line#L217: function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
```
