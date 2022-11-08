# Report


## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1] | Indexed Events | 8 |
| [GAS-2] | Structs Packing | 2 |
| [GAS-3] | ABI.ENCODE() | 5 |


### [GAS-1] Indexed Events: Use indexed events as they are less costly compared to non-indexed ones

### Instances: 1


*Instances (1)*:
```solidity
File: src/interfaces/ISizeSealed.sol

97:         event AuctionCreated(
        uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
    );

```

*Instances (2)*:
```solidity
File: src/interfaces/ISizeSealed.sol

101:         event AuctionCancelled(uint256 auctionId);

```

```

*Instances (3)*:
```solidity
File: src/interfaces/ISizeSealed.sol

103:         event Bid(
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


*Instances (4)*:
```solidity
File: src/interfaces/ISizeSealed.sol

114:        event BidCancelled(uint256 auctionId, uint256 bidIndex);


```


*Instances (5)*:
```solidity
File: src/interfaces/ISizeSealed.sol

116:        event RevealedKey(uint256 auctionId, uint256 privateKey);


```


*Instances (6)*:
```solidity
File: src/interfaces/ISizeSealed.sol

118:        event AuctionFinalized(uint256 auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);


```


*Instances (7)*:
```solidity
File: src/interfaces/ISizeSealed.sol

120:        event BidRefund(uint256 auctionId, uint256 bidIndex);


```


*Instances (8)*:
```solidity
File: src/interfaces/ISizeSealed.sol

122:        event Withdrawal(uint256 auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);


```




_____

structs packing: https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol


### [GAS-2] Structs are not packed properly, leading to more gas costs

*Instances (1)*:
```solidity
File: src/SizeSealed.sol

203:         struct FinalizeData {
        uint256 reserveQuotePerBase;
        uint128 totalBaseAmount;
        uint128 filledBase;
        uint256 previousQuotePerBase;
        uint256 previousIndex;
    }

```



*Instances (2)*:
```solidity
File: src/interfaces/ISizeSealed.sol

76:         struct AuctionParameters {
        address baseToken;
        address quoteToken;
        uint256 reserveQuotePerBase;
        uint128 totalBaseAmount;
        uint128 minimumBidQuote;
        bytes32 merkleRoot;
        ECCMath.Point pubKey;
    }

```



*Instances (3)*:
```solidity
File: src/interfaces/ISizeSealed.sol

63:         struct AuctionData {
        address seller;
        uint128 lowestBase;
        uint128 lowestQuote;
        uint256 privKey;
    }

```



### [GAS-3] ABI.ENCODE(): ABI.ENCODE() is less efficient than ABI.ENCODEPACKED()

### Instances: 5


*Instances (1)*:
```solidity
File: src/SizeSealed.sol

133:      bytes32 leaf = keccak256(abi.encodePacked(msg.sender));  
            
           

```


*Instances (2)*:
```solidity
File: src/SizeSealed.sol

467:  return keccak256(abi.encode(message));       
        
 

```



*Instances (3)*:
```solidity
File: src/SizeSealed.sol

471:   return bytes32(abi.encodePacked(baseAmount, salt));


```



*Instances (4)*:
```solidity
File: src/util/ECCMath.sol

26:   bytes memory data = abi.encode(point, scalar);


```


*Instances (5)*:
```solidity
File: src/util/ECCMath.sol

61:   return keccak256(abi.encode(point));


```


