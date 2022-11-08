## [L-01] Check that Contract Exists before using solmate's SafeTransferLib

Functions in solmate's SafeTransferLib library do not check whether a token has code at all. This responsibility is delegated to the caller.
As a call to an address with no code will be a no-op, since low-level calls to non-contracts always return true, a transfer of tokens using solmate's SafeTransferLib will succeed if the token does not have any code. Therefore, it is recommended to verify that a contract exists before using any SafeTransferLib functions. 

In the case of SIZE, this check is indirectly performed for the "baseToken" by verifying that the contract balance before and after transfer is coherent. However, no check is performed for the "quoteToken". As a result, Seller might accidentally provide a non-existing address. As a result, he will create an auction and pay GAS for an auction that no one Wille participate in.

## [L-02] Bidders are still allowed to bid after a Seller canceled the auction.

During the life of an auction, a seller is allowed to cancel an auction as long as the auction is not over (block.timestamp <  EndDate). If so, the contract logic set the EndDate to "infinite" so that the auction never end. Doing so does not induce any trouble in term of fund retrieve (bidders and seller are able to claim back their fund) but it does not prevent new bidders to bid on this canceled auction and pay GAS. 

## [L-03] No time limit on  "vestingEndDate".

SIZE contract provides the seller with the possibility to include a vesting period. However, there is no limit set on the vesting end date. As a result, a seller could set an infinite vesting end date. A bidder that did not pay attention at the vesting time will only be able to claim the cliff amount. 


## [N-01] Constants should be defined rather than using magic numbers.

Even assembly can benefit from using readable constants instead of hex/numeric literals.

***File : SizeSealed.sol***

SizeSealed.sol#L35
SizeSealed.sol#L37
SizeSealed.sol#L241
SizeSealed.sol#L249
SizeSealed.sol#L266
SizeSealed.sol#L426




## [N-02] Use of block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

***File : CommonTokenMath.sol***

CommonTokenMath.sol#L40
CommonTokenMath.sol#L41

***File : SizeSealed.sol***

SizeSealed.sol#L29
SizeSealed.sol#L31
SizeSealed.sol#L35
SizeSealed.sol#L37
SizeSealed.sol#L60
SizeSealed.sol#L425
SizeSealed.sol#L426
SizeSealed.sol#L460

## [N-03] Incomplete NatSpec

Details about some important parameters would be useful : 

https://github.com/code-423n4/2022-11-size/blob/main/src/interfaces/ISizeSealed.sol#L63-L68.




