
# QA

| | issue |
| ----------- | ----------- |
| 1 | [typo in comments](#1-typo-in-comments) |
| 2 | [event is missing indexed fields](#2-event-is-missing-indexed-fields) |
| 3 | [constants should be defined rather than using magic numbers](#3-constants-should-be-defined-rather-than-using-magic-numbers) |
| 4 | [Code Structure Deviates From Best-Practice](#4-code-structure-deviates-from-best-practice) |
| 5 | [inconsistent use of named return variables](#5-inconsistent-use-of-named-return-variables) |


## 1. typo in comments

runnning --> running
- [SizeSealed.sol#L112](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L112)

Finalises --> Finalizes
- [SizeSealed.sol#L211](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L211)

finalised --> finalized
- [SizeSealed.sol#L412](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L412)

futher --> further
- [SizeSealed.sol#L431](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L431)


## 2. event is missing indexed fields
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it’s not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

- [ISizeSealed.sol#L97](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L97)
- [ISizeSealed.sol#L103](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L103)
- [ISizeSealed.sol#L118](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L118)
- [ISizeSealed.sol#L122](https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L122)


## 3. constants should be defined rather than using magic numbers 
Even assembly can benefit from using readable constants instead of hex/numeric literals

- [SizeSealed.sol#L72](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L72)
- [SizeSealed.sol#L157](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L157)
- [SizeSealed.sol#L249](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L249)
- [SizeSealed.sol#L251](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L251)
- [SizeSealed.sol#L309](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L309)


## 4. Code Structure Deviates From Best-Practice
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier. Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private.

- [SizeSealed.sol#L203](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L203)
- [SizeSealed.sol#L217](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217)


## 5. inconsistent use of named return variables
there is an inconsistent use of named return variables in the contract some functions return named variables, others return explicit values. consider adopting a consistent approach.
this would improve both the explicitness and readability of the code, and it may also help reduce regressions during future code refactors.

most functions do not use named return

- [SizeSealed.sol#L454](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L454)
- [SizeSealed.sol#L474](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L474)
- [SizeSealed.sol#L478](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L478)
