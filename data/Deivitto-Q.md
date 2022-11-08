
# QA
# Low
## block.timestamp used as time proxy 
### Summary
Risk of using `block.timestamp` for time should be considered. 
### Details
`block.timestamp` is not an ideal proxy for time because of issues with synchronization, miner manipulation and changing block times. 

This can affect the flow of all the functions that use `atState` modifier, as:
- `bid`
- `reveal`
- `finalize`
- `refund`
- `withdraw`

This kind of issue may affect the code allowing or reverting the code before the expected deadline, modifying the normal functioning or reverting sometimes.
### References
SWC ID: 116

### Github Permalinks 
- `block.timestamp` used for decision flow is dangerous
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L29
        if (block.timestamp < a.timings.startTimestamp) {

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L31
        } else if (block.timestamp < a.timings.endTimestamp) {

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L35
        } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L37
        } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L60
        if (timings.endTimestamp <= block.timestamp) {

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L425
        if (block.timestamp >= a.timings.endTimestamp) {

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L426
            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {

### Mitigation
- Consider the risk of using `block.timestamp` as time proxy and evaluate if block numbers can be used as an approximation for the application logic. Both have risks that need to be factored in. 
- Consider using an oracle for precision

## Emitted amount can be bigger than expected
### Impact 
There are ERC20 tokens with transfer at fees. For checking if the transferred amount is the same as expected, code already compares balanceOf before and balanceOf after transfer. People can get confused in cases where real value doesn't match
### Github Permalinks
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L163-L167
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L327-L330
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L384-L387
### Mitigation
Consider implementing same system as implemented:
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L96-L105

# Informational
##  Use of magic numbers is confusing and risky
### Summary
Magic numbers are hardcoded numbers used in the code which are ambiguous to their intended purpose. These should be replaced with constants to make code more readable and maintainable.
### Details
Values are hardcoded and would be more readable and maintainable if declared as a constant

### Github Permalinks
`1e18`
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/util/CommonTokenMath.sol#L60
            uint256 cliffAmount = FixedPointMathLib.mulDivDown(baseAmount, cliffPercent, 1e18);

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L72
        if (timings.cliffPercent > 1e18) {

`1000`
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L157
        if (bidIndex >= 1000) {

`24 hours`
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L35
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L37
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L426

`256`
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L241-L251

`128`
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L266
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L309
### Mitigation
Replace magic hardcoded numbers with declared constants.




## Missing indexed event parameters
### Summary
Events without indexed event parameters make it harder and
inefficient for off-chain tools to analyze them.
### Details
Indexed parameters (“topics”) are searchable event parameters.
They are stored separately from unindexed event parameters in an efficient manner to allow for faster access. This is useful for efficient off-chain-analysis, but it is also more costly gas-wise.
 
### Github Permalinks
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L97
    event AuctionCreated(

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L101
    event AuctionCancelled(uint256 auctionId);

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L103
    event Bid(

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L114
    event BidCancelled(uint256 auctionId, uint256 bidIndex);

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L116
    event RevealedKey(uint256 auctionId, uint256 privateKey);

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L118
    event AuctionFinalized(uint256 auctionId, uint256[] bidIndices, uint256 filledBase, uint256 filledQuote);

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L120
    event BidRefund(uint256 auctionId, uint256 bidIndex);

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L122
    event Withdrawal(uint256 auctionId, uint256 bidIndex, uint256 withdrawAmount, uint256 remainingAmount);


### Mitigation
Consider which event parameters could be particularly useful to off-chain tools and should be indexed.



## Missing Natspec 
### Summary 
Missing Natspec and regular comments affect readability and maintainability of a codebase. 
### Details 
Contracts has partial or full lack of comments
### Github Permalinks 
#### Natspec @param
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L28-L43

#### Natspec @param and/or @return value
https://github.com/code-423n4/2022-11-size/blob/59ab86cffde7ba1d2565e97dd7731412a6394183/src/util/ECCMath.sol#L1-L63

#### Natspec in general 
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L466-L480

 ### mitigation
 - Add @param descriptors
 - Add @return descriptors
 - Add Natspec comments. 
 - Add inline comments. 
 - Add comments for what the contract does

## Maximum line length exceeded
### Summary
Long lines should be wrapped to conform with Solidity Style guidelines. 
### Details 
Lines that exceed the 79 (or 99) character length suggested by the Solidity Style guidelines. Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length
### Github Permalinks 


https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L98

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L118

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/interfaces/ISizeSealed.sol#L122

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/util/CommonTokenMath.sol#L65

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L94

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L99

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L144

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L163

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L166

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L187

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L217

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L269

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L297

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L325

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L336

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L358

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L377

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L409

https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L426


### Mitigation
Reduce line length to less than 99 at least to improve maintainability and readability of the code 



## Missing error messages in require statements
### Summary
Require/revert statements should include error messages in order to help at monitoring the system.
### Github Permalinks
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L40
            revert();


### Mitigation
Add error messages 

## Bad order of code
### Summary
Clearness of the code is important for the readability and maintainability.
As Solidity guidelines says about declaration order:
1.Type declarations
2.State variables
3.Events
4.Modifiers
5.Functions
Also, state variables order affects to gas in the same way as ordering structs for saving storage slots
### Github Permalink
- struct declared in the middle of functions
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L202-L209



### Mitigation
Follow solidity style guidelines https://docs.soliditylang.org/en/v0.8.16/style-guide.html

## Names are not descriptive enough
### Impact
Trying to make names shorter is sometimes helpful, however, shorting them too much would affect readability in a bad way.
### Details
In other structs this is done correctly. For example in:
`FinalizeData memory data;`
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L231
### Github Permalinks
- `a` as variable for `Auction` 
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L221

- `b` as variable for `EncryptedBid` 
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L246

### Mitigation
Consider using a more descriptive name as `auct` or `auction` for `a`
Consider using a more descriptive name as `bid` or `encryptedBid` for `b`
