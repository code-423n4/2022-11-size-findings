To avoid undefrlows, any substractions should be avoided when possible, and for those mandatory, have a require before , 4 exemples :

1/ https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L103

replaceable with same test without substraction :
`if (balanceAfterTransfer  !=  auctionParams.totalBaseAmount + balanceBeforeTransfer) `

2/ https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L371

less risky to add before `require(baseTokensAvailable >= b.baseWithdrawn )`

3/ https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L378

less risky to add before `require(b.quoteAmount >= quoteBought)`

4/ https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L386

less risky to add before `require(baseAmount >= b.baseWithdrawn)`

5/ Smalll (really small !) rounding errors occurs with high figures :
with 1_000_000 ether of quote tokens...  1 wei  lost due to roundings errors
just bid with baseAmount of 10000000000000000001 and quoteAmount of 5000001
(values from forge fuzzing)

```
Running 1 test for src/test/SizeSealedHack.t.sol:SizeSealedTest
[FAIL. Reason: Assertion failed. Counterexample: calldata=0x20eae3c80000000000000000000000000000000000000000000000008ac7230489e8000100000000000000000000000000000000000000000000000000000000004c4b41, args=[10000000000000000001, 5000001]] testHack(uint128,uint128) (runs: 53, μ: 668452, ~: 668523)
Logs:
  sellerBeforeQuote 0
  sellerAfterQuote 5000000
  quoteAmount 5000001
  Error: a == b not satisfied [uint]
    Expected: 5000001
      Actual: 5000000
  sellerBeforeBase 100000000000000000000
  sellerAfterBase 90000000000000000000
  baseAmount 10000000000000000001
  Error: a == b not satisfied [uint]
    Expected: 100000000000000000001
      Actual: 100000000000000000000
```

