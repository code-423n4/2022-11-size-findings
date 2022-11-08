## Table of Contents

- [L-01] Set a range for the duration of the auction
- [N-01] Missing Natspec
- [N-02] Event is missing indexed fields

### [L-01] Set a range for the duration of the auction

The duration of the auction is set by the auctioneer, but there is no limit to the duration. Auction can last for 1 second or 10 years. While it is not a huge problem as auctioneer can just cancel the auction and recreate a new one, it is still good to have a time range so bidders can be informed. The range need not be strict, eg 1 hour - 1 month. 

### [N-01] Missing Natspec

Missing comments for contract

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L63
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L40
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L28

### [N-02] Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields.

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/interfaces/ISizeSealed.sol#L114-L122