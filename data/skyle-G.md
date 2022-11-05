## Right shift or Left shift instead of dividing or multiplying by powers of two
### Lines
- SizeSealed.sol:241
- SizeSealed.sol:249

## Use assembly to hash instead of Solidity
### Lines
- SizeSealed.sol:133
- SizeSealed.sol:467

## Use `calldata` instead of `memory` for function arguments that do not get mutated.
Mark data types as `calldata` instead of `memory` where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as `calldata`. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies `memory` storage. 
### Lines
- SizeSealed.sol:217

## Mark functions as payable (with discretion)
You can mark public or external functions as payable to save gas. Functions that are not payable have additional logic to check if there was a value sent with a call, however, making a function payable eliminates this check. This optimization should be carefully considered due to potentially unwanted behavior when a function does not need to accept ether.
### Lines
- SizeSealed.sol:217
- SizeSealed.sol:470
- SizeSealed.sol:336
- SizeSealed.sol:55
- SizeSealed.sol:415
- SizeSealed.sol:122
- SizeSealed.sol:177
- SizeSealed.sol:391
- SizeSealed.sol:451
- SizeSealed.sol:474
- SizeSealed.sol:358
- SizeSealed.sol:466
- SizeSealed.sol:478