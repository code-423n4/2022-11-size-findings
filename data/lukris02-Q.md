# QA Report for SIZE contest
## Overview
During the audit, 6 non-critical issues were found.
№ | Title | Risk Rating  | Instance Count
--- | --- | --- | ---
NC-1 | Order of Functions | Non-Critical | 3
NC-2 | Missing NatSpec | Non-Critical | 4
NC-3 | Typos | Non-Critical | 4
NC-4 | No error message in ```revert``` | Non-Critical | 1
NC-5 | Unused named return variables | Non-Critical | 2
NC-6 | Open question | Non-Critical | 1

## Non-Critical Risk Findings(6)
### NC-1. Order of Functions
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-functions), ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.  
Functions should be grouped according to their visibility and ordered:
1) constructor
2) receive function (if exists)
3) fallback function (if exists)
4) external
5) public
6) internal
7) private
##### Instances
public functions before external:
- [```function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217) 
- [```function tokensAvailableForWithdrawal(uint256 auctionId, uint128 baseAmount)```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L451) 
- [```function computeCommitment(bytes32 message) public pure returns (bytes32) ```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L466)

##### Recommendation
Reorder functions where possible.
#
### NC-2. Missing NatSpec
##### Description
NatSpec is missing for 4 functions in 1 contracts.
##### Instances
- [```function computeCommitment(bytes32 message) public pure returns (bytes32) {```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L466) 
- [```function computeMessage(uint128 baseAmount, bytes16 salt) external pure returns (bytes32) {```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L470) 
- [```function getTimings(uint256 auctionId) external view returns (Timings memory timings) {```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L474) 
- [```function getAuctionData(uint256 auctionId) external view returns (AuctionData memory data) {```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L478) 

##### Recommendation
Add NatSpec for all functions.
#
### NC-3. Typos
##### Instances
- [```/// @notice Bid on a runnning auction```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L112) => ```running```
- [```/// @notice Finalises an auction by revealing all bids```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L211) => ```Finalizes```
- [```/// @dev Transfers `quoteToken` back to bidder and prevents bid from being finalised```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L412) => ```finalized```
- [```// Prevent any futher access to this EncryptedBid```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L431) => ```further```
#
### NC-4. No error message in ```revert```
##### Instances
- [```revert();```](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L40) 

##### Recommendation
Add error messages.
#
### NC-5. Unused named return variables
##### Description
Both named return variable(s) and return statement are used.
##### Instances
- https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L457-L463 
- https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L56

##### Recommendation
To improve clarity use only named return variables.  
For example, change:
```
function functionName() returns (uint id) {
    return x;
```
to
```
function functionName() returns (uint id) {
    id = x;
```
#
### NC-6. Open question
##### Instances
- [```/// @dev we hash the point because unsure if x,y is normal distribution (source needed)```](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L59) 