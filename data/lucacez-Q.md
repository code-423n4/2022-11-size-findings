### X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES
File: SizeSealed.sol [Line 294](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L294)
```
            data.filledBase += baseAmount;
```
File: SizeSealed.sol [Line 373](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L373)
```
        b.baseWithdrawn += baseTokensAvailable;
```

### USING PRIVATE RATHER THAN PUBLIC FOR CONSTANTS, SAVES GAS
File: ECCMath.sol [Line 8](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L8)
```
    uint256 internal constant GX = 1;
```
File: ECCMath.sol [Line 9](https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/util/ECCMath.sol#L9)
```
    uint256 internal constant GY = 2;
```
