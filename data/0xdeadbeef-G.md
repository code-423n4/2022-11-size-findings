## In the file src/SizeSealed.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)


Line 244: LOOP VARIABLES SHOULD GET CACHED: `bidIndices.length`
INCREMENTS CAN BE UNCHECKED:In Solidity 0.8+, there's a default overflow check on unsigned integers. It's possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.
AN ARRAY'S LENGTH SHOULD BE CACHED TO SAVE GAS IN FOR-LOOPS
`++I` COSTS LESS GAS THAN `I++`, ESPECIALLY WHEN IT'S USED IN `FOR`-LOOPS (`--I`/`I--` TOO)

Line 302: LOOP VARIABLES SHOULD GET CACHED: `seenBidMap.length`
INCREMENTS CAN BE UNCHECKED:In Solidity 0.8+, there's a default overflow check on unsigned integers. It's possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.
AN ARRAY'S LENGTH SHOULD BE CACHED TO SAVE GAS IN FOR-LOOPS
`++I` COSTS LESS GAS THAN `I++`, ESPECIALLY WHEN IT'S USED IN `FOR`-LOOPS (`--I`/`I--` TOO)

Line 40:            revert();
REVERT() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 241:        uint256[] memory seenBidMap = new uint256[]((bidIndices.length/256)+1);
DIVISION BY 2/4/8 SHOULD USE BIT SHIFTING

Line 249:            uint256 bitmapIndex = bidIndex / 256;
DIVISION BY 2/4/8 SHOULD USE BIT SHIFTING

Line 124:        uint128 quoteAmount,
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 196:            (uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote) =
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 197:                abi.decode(finalizeData, (uint256[], uint128, uint128));
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 205:        uint128 totalBaseAmount;
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 206:        uint128 filledBase;
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 217:    function finalize(uint256 auctionId, uint256[] memory bidIndices, uint128 clearingBase, uint128 clearingQuote)
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 266:            uint128 baseAmount = uint128(uint256(decryptedMessage >> 128));
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 319:            uint128 unsoldBase = data.totalBaseAmount - data.filledBase;
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 365:        uint128 baseAmount = b.filledBaseAmount;
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 370:        uint128 baseTokensAvailable = tokensAvailableForWithdrawal(auctionId, baseAmount);
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 451:    function tokensAvailableForWithdrawal(uint256 auctionId, uint128 baseAmount)
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 454:        returns (uint128 tokensAvailable)
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 460:            uint32(block.timestamp),
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 470:    function computeMessage(uint128 baseAmount, bytes16 salt) external pure returns (bytes32) {
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

## In the file src/util/ECCMath.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)


## In the file src/util/CommonTokenMath.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

Line 48:        uint32 vestingStart,
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 49:        uint32 vestingEnd,
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 50:        uint32 currentTime,
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 51:        uint128 cliffPercent,
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 52:        uint128 baseAmount
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 53:    ) internal pure returns (uint128) {
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Line 62:            return uint128(
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

