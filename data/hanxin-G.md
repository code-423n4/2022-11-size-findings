https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L283

If Auction has been fully filled，there is no need to continue the loop, use the 「break;」 statement instead of continue;   which will save a lot of gas
