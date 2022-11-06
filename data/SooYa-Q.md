# Withdraw 0 token when `endVesting` is equal to `startVesting`

Auction can be create by setting the `startVesting` and `endVesting` equally. Since the `startVesting` and `endVesting` is having the same number, there is one time that user will not able to withdraw their vested token. 

### Proof Of Concept
Suppose that Alice already won the bid and the auction is finalized, then Alice want to withdraw her token at `startVesting`. So she will call the `withdraw()` function. Since the `startVesting` is equal to `endVesting`, there will be no problem in it.

but if we look at the `availableTokensAtTime()`:
https://github.com/code-423n4/2022-11-size/blob/main/src/util/CommonTokenMath.sol#L54-L57
``` 
	if (currentTime > vestingEnd) {
            return baseAmount; // If vesting is over, bidder is owed all tokens
        } else if (currentTime <= vestingStart) {
            return 0; // If cliff hasn't been triggered yet, bidder receives no tokens
        } else {
            // Vesting is active and cliff has triggered
```
the function will return 0, because the current time is at the same time with `vestingStart`, so the user get 0 token rather than the `baseAmount` from the withdraw. Users will not lose their funds cause of this bug, but it just break the protocol.

### Recommend Mitigation
I think the best way to mitigate this bug is by change change the first `if` statement into:
``` if (currentTime >= vestingEnd) { ```
No need to calculate the available token when the current time is equal to `vestingEnd`, because the math will have the same number with baseAmount.


# Missing Natspec

There are missing natspec for the explanation of parameter in struct below:
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L40-L48
https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L63-L68

# Typos

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L112
"runnning" should be "running"
```@notice Bid on a runnning auction```