# QA ISSUES FOR [2022-11-SIZE](https://github.com/code-423n4/2022-11-size/tree/main/<file-path>)

## [N-01] `public` functions not called by the contract should be declared `external` instead

./src/SizeSealed.sol
```
L217:  function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
```
