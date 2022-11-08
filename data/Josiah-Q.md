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

