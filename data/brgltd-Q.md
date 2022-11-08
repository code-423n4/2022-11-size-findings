# [01] Lack of address(0) validation when creating an auction

Consider adding a check to validate `auctionParams.baseToken` and `auctionParams.quoteToken` are not address zero.

If these variables get configured with address zero, it will result in unexpected behavior for the contract.

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L92

https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L77-L78

# [02] Missing natspec/documentation

Consider adding natspec on all functions to improve documentation.

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L466-L480

# [03] Order of functions

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) recommends the following order for functions:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private

The instances bellow shows one public function between two external functions.

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L177

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L217

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L336

# [04] Inconsistent use of named return variables

Some functions return named variables, others return explicit values.

Following function returns an explicit value

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L466

Following function returns a named variable

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L475

Consider adopting a consistent approach to return values throughout the codebase by removing all named return variables, explicitly declaring them as local variables, and adding the necessary return statements where appropriate. This would improve both the explicitness and readability of the code, and it may also help reduce regressions during future code refactors.
