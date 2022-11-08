
## [G-01] ++i/i++ should be UNCHECKED{++i}/UNCHECKED{i++} when it is not possible for them to overflow (eg. in for and while loops).

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves around 40 gas per loop.

***File : SizeSealed.sol***

SizeSealed.sol#L244
SizeSealed.sol#L302




## [G-02] Usage of UINT/INT smaller than 256bits incurs overhead.

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

***File : ISizeSealed.sol***

ISizeSealed.sol#L42
ISizeSealed.sol#L43
ISizeSealed.sol#L44
ISizeSealed.sol#L65
ISizeSealed.sol#L66
ISizeSealed.sol#L80
ISizeSealed.sol#L81
ISizeSealed.sol#L107




## [G-03] Non-strict inequalities are cheaper than strict ones.

Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas). This also holds true between <= and <.

Multiple instances accros the different files.



## [G-04] "Indexing" variables inside an Event (up to 3 indexes) saves GAS.

***File : ISizeSealed.sol***

ISizeSealed.sol#L97
ISizeSealed.sol#L101
ISizeSealed.sol#L103
ISizeSealed.sol#L114
ISizeSealed.sol#L116
ISizeSealed.sol#L118
ISizeSealed.sol#L120
ISizeSealed.sol#L122

