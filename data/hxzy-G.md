1. Redundant judgment
The modifier atState of the SizeSealed contract does not pass Created and Voided anywhere in all calls. It is recommended to cancel the relevant judgment to save gas consumption.
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L30
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L38

2. Did not exit the for loop in time
In the finalize function of the SizeSealed contract, if the total amount is not enough to pay a bidder, the for loop is not jumped out after recording the amount that can be paid. It is recommended to judge whether the baseToken has been allocated at the beginning of the for loop to save gas consumption.
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L289-L291

3. Repeat check
In the finalize function of the SizeSealed contract, lines 289 and 290 ensure that filledBase will not exceed totalBaseAmount, so lines 313 and 314 are duplicate verifications and are recommended to be deleted.
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L313-L315