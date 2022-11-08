### [G-01] Else branch never executes
### [G-02] Increment variable should be unchecked

---
### Details:
#### [G-01] Else branch never executes
src/SizeSealed.sol
- https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L39-L41

Recommend eliminating `else` branch     

#### [G-02]  Increment variable should be unchecked
src/SizeSealed.sol
- https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L84

Recommend adding uncheck{} for `currentAuctionId`
