## [G-01] Can save gas on finalize

In an auction let’s say there are `1000` bids made in total and only `100` bids fill up the amount of base tokens to be sold, there is no need to run the loop on the entire `1000` bids finalize .

The check is made on [https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L283](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L283) 

```solidity
if (data.filledBase == data.totalBaseAmount) continue;
```

But the loop still runs and doesn’t break. 

It would save a lot of gas if there can be a condition in the finalize function which would break the loop as soon as filledBase == totalBaseAmount.