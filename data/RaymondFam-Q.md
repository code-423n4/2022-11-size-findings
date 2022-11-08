## Use `delete` to Clear Variables
`delete a` assigns the initial value for the type to `a`. i.e. for integers it is equivalent to `a = 0`, but it can also be used on arrays, where it assigns a dynamic array of length zero or a static array of the same length with all elements reset. For structs, it assigns a struct with all members reset. Similarly, it can also be used to set an address to zero address. It has no effect on whole mappings though (as the keys of mappings may be arbitrary and are generally unknown). However, individual keys and what they map to can be deleted: If `a` is a mapping, then `delete a[x]` will delete the value stored at x.

The delete key better conveys the intention and is also more idiomatic. Consider replacing assignments of zero with delete statements. Here are some the three instances entailed:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L432-L435
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L379
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L404

## `block.timestamp` Unreliable
The use of `block.timestamp` as part of the time checks can be slightly altered by miners/validators to favor them in contracts that have logic strongly dependent on them.

Consider taking into account this issue and warning the users that such a scenario could happen. If the alteration of timestamps cannot affect the protocol in any way, consider documenting the reasoning and writing tests enforcing that these guarantees will be preserved even if the code changes in the future.

Here are some of the instances entailed:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L29-L37
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L60
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L425-L426
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L460

## Last Array Element Omitted
The following instance of for loop is going to have the last element of the array omitted.

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L302

Consider refactoring the code line as follows:

```
        for (uint256 i; i < seenBidMap.length; i++) {
```
The following refactoring of code is another alternative albeit less gas efficient though because of the adoption of `<=` instead of `<`:

```
        for (uint256 i; i <= seenBidMap.length - 1; i++) {
```
## Variable Names
Consider making the naming of local variables more verbose and descriptive so all other peer developers would better be able to comprehend the intended statement logic, significantly enhancing the code readability. Here are some of the instances entailed:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L28
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L86
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L131
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L181
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L221
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L337-L338
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L359-L360
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L392
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L418-L419
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L456

## Un-indexed Parameters in Events
Consider indexing parameters for events, serving as logs filter when looking for specifically wanted data. Up to three parameters in an event function can receive the attribute `indexed` which will cause the respective arguments to be treated as log topics instead of data. There are the instances entailed:

https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L97-L122

## Inadequate NatSpec
Solidity contracts can use a special form of comments, i.e., the Ethereum Natural Language Specification Format (NatSpec) to provide rich documentation for functions, return variables and more. Please visit the following link for further details:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

Here are some of the instances entailed that are devoid of useful comments:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L28-L43
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L466-L480
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L28-L34
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L40-L48
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L63-L68
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L86-L91
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L97-L122

## Devoid of System Documentation and Complete Technical Specification
A system’s design specification and supporting documentation should be almost as important as the system’s implementation itself. Users rely on high-level documentation to understand the big picture of how a system works. Without spending time and effort to create palatable documentation, a user’s only resource is the code itself, something the vast majority of users cannot understand. Security assessments depend on a complete technical specification to understand the specifics of how a system works. When a behavior is not specified (or is specified incorrectly), security assessments must base their knowledge in assumptions, leading to less effective review. Maintaining and updating code relies on supporting documentation to know why the system is implemented in a specific way. If code maintainers cannot reference documentation, they must rely on memory or assistance to make high-quality changes. Currently, the only documentation for Growth DeFi is a single README file, as well as code comments.

## Early Conditional Checks
Conditional checks should appear as early as possible in a function logic. In the event of a revert, it would avoid incurring gas on code executions coming after the checks. The following code block should execute before updating `ebid`:

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L155-L159

Or, better yet, right after `Auction a` has been cached

## Inaccurate Graphical Representation
`cliffPercent` is meant to be divided by 1e18 to furnish a normalized value between 0 and 1. As such, it should be replaced with `cliffAmount` on line 31 of `CommonTokenMath.sol` to better portray the base tokens unlocked at vesting start.

https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L31

Additionally, the function logic did not implement any step wise release of vested token periodically. Hence, the graphical staircase should be replaced with a linear straight line to better portray the implication of a continuous function represented in lines 64 to 66.

https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L64-L66

## Inadequate Sanity Checks
The following code block could take in more structured checks by implementing some threshold limits:  

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L60-L74

`(timings.endTimestamp <= block.timestamp)` would easily be bypassed if `timings.endTimestamp == block.timestamp + 1`. Consider adding a reasonable amount of threshold to `block.timestamp`.

In a reverse manner, `(timings.startTimestamp >= timings.endTimestamp)` could easily be skipped if `timings.endTimestamp == timings.startTimestamp + 1` Consider also adding a reasonable amount of threshold to `timings.startTimestamp`. 

Like wise, `(timings.endTimestamp > timings.vestingStartTimestamp)` and `(timings.vestingStartTimestamp > timings.vestingEndTimestamp)` could return false by correspondingly having `(timings.endTimestamp == timings.vestingStartTimestamp)` and `(timings.vestingStartTimestamp == timings.vestingEndTimestamp)`. A threshold should also be added to `timings.endTimestamp` and `timings.vestingStartTimestamp` respectively.

As for line 72, consider refactoring it to:

```
        if (timings.cliffPercent > 1e18 || timings.cliffPercent == 0) {
```

