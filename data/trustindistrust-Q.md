## Non-critical

### Save cast value of ERC20 token instead of casting repeatedly.

[Location](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L96-L102)

#### Description

`SizeSealed.createAuction()` contains repeated casts of an address to the ERC20 type. Since the same address value is used each time, it would be more clear to save the cast ERC20 once and then use that cached value.

#### Suggested course of action

```
ERC20 baseToken = ERC20(auctionParams.baseToken);

uint256 balanceBeforeTransfer = baseToken.balanceOf(address(this));

SafeTransferLib.safeTransferFrom(baseToken, msg.sender, address(this), auctionParams.totalBaseAmount);

uint256 balanceAfterTransfer = baseToken.balanceOf(address(this));
if (balanceAfterTransfer - balanceBeforeTransfer != auctionParams.totalBaseAmount) {
    revert UnexpectedBalanceChange();
}
```

## Non-critical

### Contract overuses some custom errors, making logging and test output unncessarily harder to interpret.

#### Description 

The `SizeSealed` contract makes use of custom errors to reduce gas costs. However, the `InvalidState` error is overused in such a way as to make logging and debug/testing difficult.

For example, the `atState` modifier contains a complex filter to ensure the provided `Auction` and `States` values conform to certain restrictions before allowing execution of the guarded function. 5 branches of this filter result in the same "InvalidState" error, making it difficult to quickly ascertain which thing failed.

Further, the guarded functions themselves sometimes also contain this custom error. `bid()` `finalize()` `refund()` and `withdraw()` can emit the error, making it ambiguous as to where exactly the error is coming from if it is encountered.

`createAuction` uses `InvalidTimestamp` 5 times, and the error contains no parameter. This makes it difficult to understand which test is failing.

#### Suggested course of action

Expand upon and add distinct errors, so that only one unique error type is emitted for any given error condition or error emitter. Consider adding parameters to the errors to further disambiguate what user-provided parameter may be causing reverts.