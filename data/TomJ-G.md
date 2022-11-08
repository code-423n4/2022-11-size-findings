## Table of Contents
- Should Use Unchecked Block where Over/Underflow is not Possible
- Defined Variables Used Only Once
- X = X + Y costs less gass than X += Y
- Internal Function Called Only Once can be Inlined
- Using Elements Smaller than 32 bytes (256 bits) Might Use More Gas

&ensp;
### Should Use Unchecked Block where Over/Underflow is not Possible

#### Issue
Since Solidity 0.8.0, all arithmetic operations revert on overflow and underflow by default.
However in places where overflow and underflow is not possible, it is better to use unchecked block to reduce the gas usage.
Reference: https://docs.soliditylang.org/en/v0.8.15/control-structures.html#checked-or-unchecked-arithmetic

#### PoC
1. finalize() of SizeSealed.sol
```
Wrap line 319 with unchecked since underflow is not possible due to line 313-315 check
313:        if (data.filledBase > data.totalBaseAmount) {
314:            revert InvalidState();
315:        }
319:            uint128 unsoldBase = data.totalBaseAmount - data.filledBase;
```
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L313-L319

#### Mitigation
Wrap the code with uncheck like described in above PoC. 

&ensp;
### Defined Variables Used Only Once

#### Issue
Certain variables is defined even though they are used only once.
Remove these unnecessary variables to save gas.
For cases where it will reduce the readability, one can use comments to help describe
what the code is doing.

#### PoC
1. Remove `leaf` variable of bid() of SizeSealed.sol 
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L133-L134
Mitigation:
Delete line 133 and replace line 134 with below code
```
            if (!MerkleProofLib.verify(proof, a.params.merkleRoot, keccak256(abi.encodePacked(msg.sender)))) {
```

2. Remove `unsoldBase` variable of finalize() of SizeSealed.sol 
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L319-L321
Mitigation:
Delete line 319 and replace line 321 with below code
```
            SafeTransferLib.safeTransfer(ERC20(a.params.baseToken), a.data.seller, data.totalBaseAmount - data.filledBase);
```

3. Remove `refundedQuote` variable of withdraw() of SizeSealed.sol 
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L378-L381
Mitigation:
Delete line 378 and replace line 381 with below code
```
            SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount - quoteBought);
```

4. Remove `data` variable of ecMul() of ECCMath.sol 
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L26-L28
Mitigation:
Delete line 26 and replace line 28 with below code
```
        (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(abi.encode(point, scalar));
```

5. Remove `sharedKey` variable of encryptMessage() of ECCMath.sol 
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L43-L44
Mitigation:
Delete line 43 and replace line 44 with below code
```
        encryptedMessage = message ^ hashPoint(sharedPoint);
```

#### Mitigation
Do not define variable that is used only once.
Details are listed on above PoC.

&ensp;
### X = X + Y costs less gass than X += Y

#### Issue
X = X + Y costs less gass than X += Y for state variables

#### PoC
Total of 1 instance found.
```solidity
SizeSealed.sol:373:        b.baseWithdrawn += baseTokensAvailable;
```
Change to
```solidity
SizeSealed.sol:373:        b.baseWithdrawn = b.baseWithdrawn + baseTokensAvailable;
```

&ensp;
### Internal Function Called Only Once Can be Inlined

#### Issue
Certain function is defined even though it is called only once.
Inline it instead to where it is called to avoid usage of extra gas.

#### PoC
Total of 1 instance found.

1. tokensAvailableAtTime() of CommonTokenMath.sol
This function called only once at SizeSealed.sol(line 457)
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/CommonTokenMath.sol#L47
https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L457

#### Mitigation
I recommend to not define above functions and instead inline it at place it is called.

&ensp;
### Using Elements Smaller than 32 bytes (256 bits) Might Use More Gas

#### Issue
Since EVM operates on 32 bytes at a time, if the element is smaller than that, the EVM must use more operations
in order to reduce the elements from 32 bytes to specified size. Therefore it is more gas saving to use 32 bytes 
unless it is used in a struct or state variable in order to reduce storage slot. 

Reference: https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html

#### PoC
Total of 13 instances found.
```
CommonTokenMath.sol:48:        uint32 vestingStart,
CommonTokenMath.sol:49:        uint32 vestingEnd,
CommonTokenMath.sol:50:        uint32 currentTime,
CommonTokenMath.sol:51:        uint128 cliffPercent,
CommonTokenMath.sol:52:        uint128 baseAmount
CommonTokenMath.sol:53:    ) internal pure returns (uint128) {
SizeSealed.sol:124:        uint128 quoteAmount,
SizeSealed.sol:319:            uint128 unsoldBase = data.totalBaseAmount - data.filledBase;
SizeSealed.sol:365:        uint128 baseAmount = b.filledBaseAmount;
SizeSealed.sol:370:        uint128 baseTokensAvailable = tokensAvailableForWithdrawal(auctionId, baseAmount);
SizeSealed.sol:451:    function tokensAvailableForWithdrawal(uint256 auctionId, uint128 baseAmount)
SizeSealed.sol:454:        returns (uint128 tokensAvailable)
SizeSealed.sol:470:    function computeMessage(uint128 baseAmount, bytes16 salt) external pure returns (bytes32) {
```

#### Mitigation
I suggest using uint256 instead of anything smaller and downcast where needed.

&ensp;