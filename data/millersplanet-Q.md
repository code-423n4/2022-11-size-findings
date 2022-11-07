# QA Report

## [QA - 01]

There are come inconsitencies with `if` statements within the code base. Some of them are declared as one-liners without brackets, e.g:
`if (_state != States.Created) revert InvalidState();` (SizeSealed.sol#30), while some others are declared with brackets, occupying 3 lines, where they could follow the same styling as in the rest of the codebase, e.g. (SizeSealed.sol#366):

```
if (baseAmount == 0) {
		revert InvalidState();
}
``` 

It's recommended to maintain a consistent styling along the project code base. For improving readability and reduce lines of code, remove the brackets and declare the `if` statements of the following lines as one-liners: 

Instances (21): ``SizeSealed.sol#60, 63, 66, 69, 72, 134, 140, 157, 182, 223, 227, 303, 309, 313, 339, 343, 361, 366, 393, 398, 420``


## [QA - 02]


There are some missing NATSPEC format comments on ``ISizeSealed.sol``. While the structs ``Timings`` and ``AuctionParameters`` have well defined NATSPEC comments, the structs ``EncryptedBid``, ``AuctionData`` and ``Auction`` don't have any.


It's recommended to finish writing the proper comments for helping developers and users comprehend more clearly and quickly the data of the mentioned structs and its usage inside the protocol.

Instances (3): ``ISizeSealed.sol#40, 63, 86``
