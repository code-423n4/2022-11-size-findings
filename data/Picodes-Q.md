### [NC-1] - Typos

[Here](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L112) runnning -> running

### [NC-2] Unnecessary check

```solidity
if (data.filledBase > data.totalBaseAmount) {
    revert InvalidState();
}
```

The [above check](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L313) in `finalize` is useless as there is no way `data.filledBase` goes above `data.totalBaseAmount` due to [these lines](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L289)
