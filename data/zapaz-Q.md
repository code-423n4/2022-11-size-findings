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
