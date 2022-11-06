

### [G01] State variables only set in the constructor should be declared `immutable`

#### Impact
Avoids a Gusset (20000 gas)
#### Findings:
```
2022-11-size/src/SizeSealed.sol::20 => uint256 public currentAuctionId;
```




### [G02] `<array>.length` should not be looked up in every loop of a `for` loop

#### Impact
Even memory arrays incur the overhead of bit tests and bit shifts to 
calculate the array length. Storage array length checks incur an extra 
Gwarmaccess (100 gas) PER-LOOP.

#### Findings:
```
2022-11-size/src/SizeSealed.sol::244 => for (uint256 i; i < bidIndices.length; i++) {
```




### [G03] `++i/i++` should be `unchecked{++i}`/`unchecked{++i}` when it is not possible for them to overflow, as is the case when used in `for` and `while` loops


#### Findings:
```
2022-11-size/src/SizeSealed.sol::244 => for (uint256 i; i < bidIndices.length; i++) {
2022-11-size/src/SizeSealed.sol::302 => for (uint256 i; i < seenBidMap.length - 1; i++) {
```


### [G04] It costs more gas to initialize variables to zero than to let the default of zero be applied


#### Findings:
```
2022-11-size/src/SizeSealed.sol::379 => b.quoteAmount = 0;
2022-11-size/src/SizeSealed.sol::435 => b.commitment = 0;
```




### [G05] `++i` costs less gas than `i++`, especially when it’s used in forloops (`--i`/`i--` too)


#### Findings:
```
2022-11-size/src/SizeSealed.sol::244 => for (uint256 i; i < bidIndices.length; i++) {
2022-11-size/src/SizeSealed.sol::302 => for (uint256 i; i < seenBidMap.length - 1; i++) {
```


###  [G06] Duplicated `require()`/`revert()` checks should be refactored to a modifier or function


#### Findings:
```
2022-11-size/src/SizeSealed.sol::61 => revert InvalidTimestamp();
2022-11-size/src/SizeSealed.sol::141 => revert UnauthorizedCaller();
2022-11-size/src/SizeSealed.sol::158 => revert InvalidState();
2022-11-size/src/SizeSealed.sol::188 => revert InvalidPrivateKey();
2022-11-size/src/SizeSealed.sol::228 => revert InvalidCalldata();
2022-11-size/src/SizeSealed.sol::304 => revert InvalidState();
```


### [G07] Use custom errors rather than `revert()`/`require()` strings to save deployment gas


#### Findings:
```
2022-11-size/src/SizeSealed.sol::61 => revert InvalidTimestamp();
2022-11-size/src/SizeSealed.sol::64 => revert InvalidTimestamp();
2022-11-size/src/SizeSealed.sol::67 => revert InvalidTimestamp();
2022-11-size/src/SizeSealed.sol::70 => revert InvalidTimestamp();
2022-11-size/src/SizeSealed.sol::73 => revert InvalidCliffPercent();
2022-11-size/src/SizeSealed.sol::81 => revert InvalidReserve();
2022-11-size/src/SizeSealed.sol::104 => revert UnexpectedBalanceChange();
2022-11-size/src/SizeSealed.sol::135 => revert InvalidProof();
2022-11-size/src/SizeSealed.sol::141 => revert UnauthorizedCaller();
2022-11-size/src/SizeSealed.sol::145 => revert InvalidBidAmount();
2022-11-size/src/SizeSealed.sol::158 => revert InvalidState();
2022-11-size/src/SizeSealed.sol::183 => revert UnauthorizedCaller();
2022-11-size/src/SizeSealed.sol::188 => revert InvalidPrivateKey();
2022-11-size/src/SizeSealed.sol::224 => revert InvalidPrivateKey();
2022-11-size/src/SizeSealed.sol::228 => revert InvalidCalldata();
2022-11-size/src/SizeSealed.sol::275 => revert InvalidSorting();
2022-11-size/src/SizeSealed.sol::298 => revert InvalidCalldata();
2022-11-size/src/SizeSealed.sol::304 => revert InvalidState();
2022-11-size/src/SizeSealed.sol::310 => revert InvalidState();
2022-11-size/src/SizeSealed.sol::314 => revert InvalidState();
2022-11-size/src/SizeSealed.sol::340 => revert UnauthorizedCaller();
2022-11-size/src/SizeSealed.sol::344 => revert InvalidState();
2022-11-size/src/SizeSealed.sol::362 => revert UnauthorizedCaller();
2022-11-size/src/SizeSealed.sol::367 => revert InvalidState();
2022-11-size/src/SizeSealed.sol::394 => revert UnauthorizedCaller();
2022-11-size/src/SizeSealed.sol::399 => revert InvalidState();
2022-11-size/src/SizeSealed.sol::421 => revert UnauthorizedCaller();
2022-11-size/src/SizeSealed.sol::427 => revert InvalidState();
```


### [G08] Use `calldata` instead of `memory` for function parameters

#### Impact
Use calldata instead of memory for function parameters. Having function arguments use calldata instead of memory can save gas.
#### Findings:
```

2022-11-size/src/SizeSealed.sol::217 => function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
2022-11-size/src/util/ECCMath.sol::25 => function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {
2022-11-size/src/util/ECCMath.sol::37 => function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)
2022-11-size/src/util/ECCMath.sol::51 => function decryptMessage(Point memory sharedPoint, bytes32 encryptedMessage)
2022-11-size/src/util/ECCMath.sol::60 => function hashPoint(Point memory point) internal pure returns (bytes32) {
```


### [G09] Amounts should be checked for 0 before calling a transfer

#### Impact
Checking non-zero transfer values can avoid an expensive external call and save gas.

While this is done in some places, it’s not consistently done in the solution.

I suggest adding a non-zero-value check here:
#### Findings:
```
2022-11-size/src/SizeSealed.sol::321 => SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), a.data.seller, unsoldBase);
2022-11-size/src/SizeSealed.sol::327 => SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), a.data.seller, filledQuote);
2022-11-size/src/SizeSealed.sol::351 => SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount);
2022-11-size/src/SizeSealed.sol::381 => SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, refundedQuote);
2022-11-size/src/SizeSealed.sol::384 => SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), msg.sender, baseTokensAvailable);
2022-11-size/src/SizeSealed.sol::409 => SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), msg.sender, a.params.totalBaseAmount);
2022-11-size/src/SizeSealed.sol::439 => SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount);
```



### [G10] `abi.encode()` is less efficient than abi.encodePacked()


#### Findings:
```
2022-11-size/src/SizeSealed.sol::467 => return keccak256(abi.encode(message));
2022-11-size/src/SizeSealed.sol::471 => return bytes32(abi.encodePacked(baseAmount, salt));
2022-11-size/src/util/ECCMath.sol::26 => bytes memory data = abi.encode(point, scalar);
2022-11-size/src/util/ECCMath.sol::61 => return keccak256(abi.encode(point));
```




### [G11] <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES


#### Findings:
```
2022-11-size/src/SizeSealed.sol::294 => data.filledBase += baseAmount;
2022-11-size/src/SizeSealed.sol::373 => b.baseWithdrawn += baseTokensAvailable;
```






### [G12] STORAGE POINTER TO A STRUCTURE IS CHEAPER THAN COPYING EACH VALUE OF THE STRUCTURE INTO MEMORY, SAME FOR ARRAY AND MAPPING (5 INSTANCES)

#### Impact
It may not be obvious, but every time you copy a storage struct/array/mapping to a memory variable, you are copying each member by reading it from storage, which is expensive. And when you use the storage keyword, you are just storing a pointer to the storage, which is much cheaper. Exception: case when you need to read all or many members multiple times. In report included only cases that saved gas
#### Findings:
```
2022-11-size/src/SizeSealed.sol::186 => ECCMath.Point memory pubKey = ECCMath.publicKey(privateKey);
2022-11-size/src/SizeSealed.sol::256 => ECCMath.Point memory sharedPoint = ECCMath.ecMul(b.pubKey, sellerPriv);
2022-11-size/src/ECCMath.sol::42 => Point memory sharedPoint = ecMul(encryptToPub, encryptWithPriv);
```


