1)++i/i++ should be unchecked{++i}/unchecked{++i} when it is not possible for them to overflow,
as is the case when used in for- and while-loops


File: 2022-11-size\src\SizeSealed.sol
  244,9:         for (uint256 i; i < bidIndices.length; i++) {
  302,9:         for (uint256 i; i < seenBidMap.length - 1; i++) {
  
  
  
2)X = X + Y IS CHEAPER THAN X += Y  

File: 2022-11-size\src\SizeSealed.sol
  294,29:             data.filledBase += baseAmount;
  373,25:         b.baseWithdrawn += baseTokensAvailable;
  
  
