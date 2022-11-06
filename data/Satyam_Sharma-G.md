1.Mark address as indexed in events can save gas

event AuctionCreated(
        uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
    );
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L97

https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L103

2.Mark constant variable as private save gas

uint256 internal constant GX = 1;
    uint256 internal constant GY = 2;
https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L8

3.Declare currentAuctionId variable inside function:

As currentAuctionId is used one time in whole contract inside function createAuction, declaring it in storage rather than local state will cost much gas than using directly inside a function.
   uint256 public currentAuctionId;
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L20

        uint256 auctionId = ++currentAuctionId;
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L84

pragma solidity ^0.8.0;

contract A {
    uint Svar;
    function check() public   {
        
        uint increase = ++Svar;
    }

    
}// 108343 

contract B{
    
    function check() public view  {
        uint Svar;
        uint increase = ++Svar;
    }

}//107283
Tool use: Remix

Recommendation: using currentAuctionId inside a function that using this variable will save a unrequired gas wastage. 