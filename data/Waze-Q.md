## [N-01] Missing indexed field
## code snipped
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L98
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L101
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L103
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L114
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L116
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L118
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L120
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/interfaces/ISizeSealed.sol#L122
## recommendation
event is missing indexed fields in important parameter. we recommend indexed at important field for increase creadibility.

## [N-02] Natspec incomplete
## code snipped
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/util/ECCMath.sol#L18
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/util/ECCMath.sol#L25
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/util/ECCMath.sol#L60
https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L466-L479
## recommendation
the file has a natspec comment  to explain utility about function or parameter. but in these file the natspec comment incomplete. So we recommend to complete the natspec comment to increase readability and make it easier when there is an audit.