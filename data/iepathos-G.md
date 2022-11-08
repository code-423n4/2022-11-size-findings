### G-01 Use > rather than >= where possible to avoid extra opcode used in >=. Corollary, use < rather than <= where possible to avoid extra opcode used in <=.

For example, SizeSealed.sol line 157 `bidIndex >= 1000` should be `bidIndex > 999`

This one change reduces deployment cost by 200 gas and deployment size by 1 and will save some gas whenever bid is called.  Similar gains can be made by applying the same kind of optimization to the following lines below that are using >= or <=

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L63
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L157
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L270
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L425

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L35
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L60
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L426

https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L56