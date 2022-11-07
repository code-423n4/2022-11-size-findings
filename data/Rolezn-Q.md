## Summary<a name="Summary">

### Non-critical Issues
| |Issue|Contexts|
|-|:-|:-:|
| [NC&#x2011;1](#NC&#x2011;1) | Event Is Missing Indexed Fields | 7 |
| [NC&#x2011;2](#NC&#x2011;2) | `States.Voided` is never used | 1 |

Total: 8 contexts over 2 issues

## Low Risk Issues

## Non Critical Issues

### <a href="#Summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> Event Is Missing Indexed Fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). 

Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

#### <ins>Proof Of Concept</ins>


```
event AuctionCreated(
        uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
    );
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L97

```
event Bid(
        address sender,
        uint256 auctionId,
        uint256 bidIndex,
        uint128 quoteAmount,
        bytes32 commitment,
        ECCMath.Point pubKey,
        bytes32 encryptedMessage,
        bytes encryptedPrivateKey
    );
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L103

```
event BidCancelled(uint256 auctionId, uint256 bidIndex);
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L114

```
event RevealedKey(uint256 auctionId, uint256 privateKey);
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L116

```
event AuctionFinalized(uint256 auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L118

```
event BidRefund(uint256 auctionId, uint256 bidIndex);
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L120

```
event Withdrawal(uint256 auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);
```

https://github.com/debtdao/Line-of-Credit/2022-11-size/tree/main/src/interfaces/ISizeSealed.sol#L122





### <a href="#Summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> `States.Voided` is never used

The state of `Voided` is never used in the `SizeSealed.sol` contract.

#### <ins>Proof Of Concept</ins>


```
    Voided,
```
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L32


