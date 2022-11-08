QA 1: https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L148-L159

If the bidder is already equal to 1000, there is no need to declare the EncryptedBid memory ebid variable

Put the declaration and assignment of the EncryptedBid memory ebid variable after the judgment statement, you can save some gas, although not much, but it is necessary to develop good habits

QA 2 : https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L98

Transaction using SafeTransferLib.safeTransferFrom, but none of any  functions  check that a token has code at all!  

QA 3 : https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L163-L167

CompilerError: Stack too deep. Try compiling with `--via-ir` (cli) or the equivalent `viaIR: true` (standard JSON) while enabling the optimizer. Otherwise, try removing local variables.   







