# Report
## Non-Critical Issues ##
### [N-1]: Function defines a named return variable but then it uses return statements
**Context:**

1. https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L457-L463 
1. https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L56 

**Recommendation:**

Choose named return variable or return statement. It is unnecessary to use both.

### [N-2]: Provide an error message for revert
**Context:**

```revert();``` [L40](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L40) 

### [N-3]: Wrong order of functions
**Context:**

1. [L203](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L203) (struct can not go after external function)
1. [SizeSealed.finalize](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217) (all public functions must be after all external functions)
1. [SizeSealed.tokensAvailableForWithdrawal](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L451) (all public functions must be after all external functions)
1. [SizeSealed.computeCommitment](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L466) (all public functions must be after all external functions)

**Description:**

According to [official solidity documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) functions should be grouped according to their visibility and ordered:

+ constructor

+ receive function (if exists)

+ fallback function (if exists)

+ external

+ public

+ internal

+ private

Within a grouping, place the view and pure functions last.

**Recommendation:**

Put the functions in the correct order according to the documentation.

### [N-4]: NatSpec is missing
**Context:**

1. https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L28
1. https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L203
1. https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L466
1. https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L470
1. https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L474 
1. https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L478 

### [N-5]: Typos
**Context:**

1. ```/// @notice Bid on a runnning auction``` [L112](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L112) (Change ***runnning*** to ***running***)
1. ```/// @notice Finalises an auction by revealing all bids``` [L211](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L211) (Change ***Finalises*** to ***Finalizes***)
1. ```/// @dev Transfers `quoteToken` back to bidder and prevents bid from being finalised``` [L412](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L412) (Change ***finalised*** to ***finalized***)
1. ```// Prevent any futher access to this EncryptedBid``` [L431](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L431) (Change ***futher*** to ***further***)

### [N-6]: Natspec is incomplete
**Context:**

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L177 (@param auctionId is missing)
