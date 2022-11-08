# 2022-11-SIZE
## Low Risk And Non-Critical Issues

### `public` functions not called by the contract should be declared `external` instead


_There are **1** instances of this issue:_

```solidity
File: src/SizeSealed.sol

217:  function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
```
https://github.com/code-423n4/2022-11-size/tree/main/src/SizeSealed.sol

