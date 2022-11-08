## unnecessary balance before/after check
    
```solidity
SafeTransferLib.safeTransferFrom(
    ERC20(auctionParams.baseToken), msg.sender, address(this), auctionParams.totalBaseAmount
);

uint256 balanceAfterTransfer = ERC20(auctionParams.baseToken).balanceOf(address(this));

if (balanceAfterTransfer - balanceBeforeTransfer != auctionParams.totalBaseAmount) {
```

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol/#L94](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol/#L94)

SafeTransferLib.safeTransferFrom will revert if the tokens are not transferred. This means the `if (balanceAfterTransfer - balanceBeforeTransfer != auctionParams.totalBaseAmount)` check is not necessary.

## Unnecessary looping in `finalize` if base abount has been filled

```solidity

for (uint256 i; i < bidIndices.length; i++) {
    if (data.filledBase == data.totalBaseAmount) continue;
    ...
    data.filledBase += baseAmount;
```

[https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol/#L283](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol/#L283)

The `data.filledBase` if set after the if check only. `data.totalBaseAmount` is not changed in the loop. This means that if this if case is true once, it will be true for all future iterations. Therefore you can use `break` instead of `continue`

```solidity
for (uint256 i; i < bidIndices.length; i++) {
    if (data.filledBase == data.totalBaseAmount) break;
```