## USE OF BLOCK.TIMESTAMP
Some contract code uses the block.timestamp as part of the calculations and time checks. Nevertheless, timestamps can be slightly altered by miners/validators to favor them in contracts that have logics that depend strongly on them.

Consider taking into account this issue and warning the users that such a scenario could happen. If the alteration of timestamps cannot affect the protocol in any way, consider documenting the reasoning and writing tests enforcing that these guarantees will be preserved even if the code changes in the future. Here are the instances found.

[Lines 29 - 37](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L29-L37)
[Line 60](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L60)
[Lines 425 - 426](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L425-L426)

## EVENT PARAMETERS SHOULD BE INDEXED
Up to three event parameters should be indexed. This will help filter off the logs in listening for specifically wanted data. Using `indexed` has the benefit of making the arguments log topics instead of data. There are seven instances found.

[Lines 97 -122](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L97-L122)

## USE OF DESCRIPTIVE VARIABLE NAMES
Non-descriptive local variables could make code base difficult to read and navigate. Here are some of the instance found.

[Line 86](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L86)
[line 181](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L181)
[Lines 337 - 338](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L337-L338)
[Lines 359 - 360](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L359-L360)
[Lines 418 - 419](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L418-L419)

## SETTING TO DEFAULT VALUES
As documented in the link:

https://docs.soliditylang.org/en/v0.8.17/types.html?highlight=delete#delete

It is recommended using `delete` whenever there is a need to set state variable to its default value, which has a good side effect of saving gas. Here are the instances found.

[Line 379](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L379)
[Line 404](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L404)
[Lines 432 - 435](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L432-L435)

## DOS PREVENTION COULD BE EXPLOITED
It is intuitive to limit the bids of an auction to 1000 to prevent DOS as commented on [Line 156](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L156). However, without adequate measures, the following conditional check could be exploited by a malicious bidder.

[Line 157 - 159](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L157-L159)

```
        if (bidIndex >= 1000) {
            revert InvalidState();
        }
```
This is because a bidder can keep invoking `bid()` and `cancelBid()` until `bid()` begins to revert.

As such, it is recommended that all cancelled bids be removed from the array and retain only the valid ones. One easy way is to have the cancelled bid assigned with the last bid prior to having the last element removed using `array.pop()`. 

## INCOMPLETE FOR LOOP
The for loop below does not have the last element catered for.

[Line 302 - 306](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302-L306)

It can be fixed by just having line 302 refactored to:

```
        for (uint256 i; i < seenBidMap.length; i++) {
```
## MISSING NATSPEC
As documented in the link below:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

It is recommended using a special form of comments, i.e., the Ethereum Natural Language Specification Format (NatSpec) to provide rich documentation for functions, return variables and more.

Here are some of the instance found.

[Lines 40 - 48](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L40-L48)
[Lines 97 - 122](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L97-L122)
[Lines 28 - 43](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L28-L43)
[Lines 466 - 480](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L466-L480)

## INSUFFICIENT DOCUMENTATION
The code base lacks code documentation, high-level descriptions, and examples, making the contracts difficult to review and increasing the likelihood of user mistakes. The documentation would benefit from more detail.

Review and properly document the above mentioned aspects of the code base. And, consider writing a formal specification of the protocol.

## THRESHOLD LIMIT CHECKS
Certain parameters of the contracts can be configured to edge/extreme values, causing a variety of issues and breaking expected interactions within/between contracts. Here are the six instances found pertaining to critical parameter that lacks  robust sanity/threshold/limit checks.

[Lines 60 - 82](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L60-L82)

For instance, the check would pass if `timings.endTimestamp` is set to only one second greater than `block.timestamp` in line 60. This could have been prevented if a proper threshold value had been to `timings.endTimestamp`.

## CONDITIONAL CHECKS
Checks should be done as early as possible in a code block to prevent unnecessary code executions prior to it that would not only be reverted but also incur a wastage of gas. Here is one instance found that should at least be inserted before line 148.

[Line 155 - 159](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L155-L159)   

## SPELLING CORRECTIONS

[Line 112](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L112)

```
    @ runnning should be corrected to running
    /// @notice Bid on a runnning auction
```
[Line 431](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L431)

```
    @ futher should be corrected further
        // Prevent any futher access to this EncryptedBid
```
