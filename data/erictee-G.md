### [G-01] ```abi.encode()``` is less efficient than ```abi.encodePacked()```


#### Impact
Consider using ```abi.encodePacked()``` instead for efficieny.


#### Findings:
```
2022-11-size/src/SizeSealed.sol:L467        return keccak256(abi.encode(message));

2022-11-size/src/util/ECCMath.sol:L26        bytes memory data = abi.encode(point, scalar);

2022-11-size/src/util/ECCMath.sol:L61        return keccak256(abi.encode(point));

2022-11-size/src/test/SizeSealed.t.sol:L462        auction.reveal(aid, 1, abi.encode(bidIndices, 1 ether, 5e6));

2022-11-size/src/test/mocks/MockSeller.sol:L97        auctionContract.reveal(auctionId, SELLER_PRIVATE_KEY, abi.encode(bidIndices, clearingBase, clearingQuote));

2022-11-size/src/test/mocks/MockBuyer.sol:L34        salt = bytes16(keccak256(abi.encode("randomsalt")));

```

### [G-02] Explicit initialization with zero not required


#### Impact
Explicit initialization with zero is not required for variable declaration because uints are 0 by default. Removing this will reduce contract size and save a bit of gas.


#### Findings:
```
2022-11-size/src/test/SizeSealedFuzz.t.sol:L170        uint256 expectedBase = 0;

```

### [G-03] ```++i```/```i++``` should be ```unchecked{++i}```/```unchecked{i++}``` when it is not possible for them to overflow, as is the case when used in for- and while-loops.


#### Impact
The ```unchecked``` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop.


#### Findings:
```
2022-11-size/src/SizeSealed.sol:L244        for (uint256 i; i < bidIndices.length; i++) {

2022-11-size/src/SizeSealed.sol:L302        for (uint256 i; i < seenBidMap.length - 1; i++) {

```
