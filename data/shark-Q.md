## Use `delete` to reset variables

The reason why is because `delete` keyword better communicates the intention of what you are trying to do.

For example:

File: `SizeSealed.sol` [Line 404](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L404)

```
a.data.seller = address(0);
```

The above could use the `delete` keyword like so:

```
delete a.data.seller; // resets to default value, for type address that would be address(0)
```

Here are the rest of the instances:

File: `SizeSealed.sol` [Line 347](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L347)
File: `SizeSealed.sol` [Line 379](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L379)
File: `SizeSealed.sol` [Line 432](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L432)
File: `SizeSealed.sol` [Line 435](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L435)

## Revert as early as possible

Conditional checks should appear as early as possible in a function. If a revert were to occur, it would avoid incurring extra gas on code coming after the checks.

The following lines of code can be executed before updating `ebid`:

File: `SizeSealed.sol` [Line 155-159](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L155-L159)

## Typos

File: SizeSealed.sol ([Line 112](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L112), [Line 211](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L211), [Line 412](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L412), [Line 431](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L431))

```
      /// @audit Change "runnning" to "running"
112:  /// @notice Bid on a runnning auction

      /// @audit To be consistent, only use one spelling of the word.
      /// @audit "Finalises"
211:  /// @notice Finalises an auction by revealing all bids

      /// @audit "finalised"
412:  /// @dev Transfers `quoteToken` back to bidder and prevents bid from being finalised

      /// @audit Change "futher" to "further"
431:  // Prevent any futher access to this EncryptedBid
```
