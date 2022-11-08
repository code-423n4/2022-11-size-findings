[G-01] X = X + Y IS CHEAPER THAN X += Y;

Usually does not work with struct and mappings

   
    data.filledBase += baseAmount;
   

fix:

    data.filledBase = data.filledBase + baseAmount;

instances:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L294-L373


[G-01.1] <x> = <x> + 1 even more efficient than pre increment.

    uint256 auctionId = ++currentAuctionId;

FIX:
    
    uint256 auctionId = currentAuctionId + 1;

instances:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L84-L



