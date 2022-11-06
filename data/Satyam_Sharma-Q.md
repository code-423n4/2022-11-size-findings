1.In ECCMath.ecMul first check the condition than encode:

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L25-L31
bytes memory data = abi.encode(point, scalar);
        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);

here, sclar is checked after abi.encode will affect the data over here:
 (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(data);

 Recommendation: first check than encode.

2.In CommonTokenMath.tokensAvailableAtTime Make a necessary check:

https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L52
On tokensAvailableAtTime make a check on variable uint128 baseAmount, so that it never be equal to zero in any condition, if it occur to be zero calculation in cliffAmount will get affected.

https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L60
uint256 cliffAmount = FixedPointMathLib.mulDivDown(baseAmount, cliffPercent, 1e18);