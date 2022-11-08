# QA Report
## Found NC [1]

### [NC-01] Magic Number Used

Findings [1]:

*src/SizeSealed.sol*
Line(s): [157](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L157)

Although defined in a comment on [L156](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L156) a magic number `1000` is used. Consider making a variable `MAX_BID_INDEX` in `1000`'s place.

```solidity
156:	// Max of 1000 bids on an auction to prevent DOS
157:	if (bidIndex >= 1000) {
158:		revert InvalidState();
159:	}
```