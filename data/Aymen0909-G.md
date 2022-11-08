# Gas Optimizations

## Findings

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1      | Using `storage` pointer to a struct cost more gas tham `memory` when it's values are called multiple times |  3 |
| 2      | `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables  |  1 |
| 3      | Use `calldata` instead of `memory` for function parameters type  |  1 |
| 4      | `++i/i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops |  2  |

## Findings

### 1- Using `storage` pointer to a struct cost more gas tham `memory` when it's values are called multiple times :

In general using the storage keyword instead of memory for reading from struct/array/mapping cost less gas, but this is only true if you don't need to read all or many members multiple times.

There are 3 instances of this issue:

File: src/SizeSealed.sol [Line 181](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L181)

```
Auction storage a = idToAuction[auctionId];
```

File: src/SizeSealed.sol [Line 359](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L359)

```
Auction storage a = idToAuction[auctionId];
```

File: src/SizeSealed.sol [Line 418](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L418)

```
Auction storage a = idToAuction[auctionId];
```


### 2- `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables :

Using the addition operator instead of plus-equals saves **113 gas** as explained [here](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8)

There is 1 instance of this issue:

File: src/SizeSealed.sol [Line 373](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L373)
```
b.baseWithdrawn += baseTokensAvailable;
```

### 3- Use `calldata` instead of `memory` for function parameters type :

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory.

There is 1 instance of this issue:

File: src/SizeSealed.sol [Line 217](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217)
```
function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
```

### 4- `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops :

There are 2 instances of this issue:

File: src/SizeSealed.sol [Line 244](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L244)
```
for (uint256 i; i < bidIndices.length; i++)
```

File: src/SizeSealed.sol [Line 302](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302)
```
for (uint256 i; i < seenBidMap.length - 1; i++)
```