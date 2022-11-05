 USING PRIVATE RATHER THAN PUBLIC FOR CONSTANTS, SAVES GAS

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table


-SizeSealed.sol line 20


++I COSTS LESS GAS THAN I++, ESPECIALLY WHEN IT’S USED IN FOR-LOOPS (--I/I-- TOO)

this saves 6 gas per loop

4 instances
line 244, 302 in SizeSealed.sol

GET FUNCTIONS SHOULD ACCEPT MORE THAN 1 ARGUMENT:

Consider the following scenario: a user wants to call getAuctionData() with five different inputs. In the streamlined form, the user would only need to pay the transaction's fixed gas cost and the gas for the msg.sender check once.

2 instances
line 474, 478 in SizeSealed.sol
