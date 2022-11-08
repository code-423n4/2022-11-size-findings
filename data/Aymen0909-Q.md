# QA Report

## Summary

|               | Issue         | Risk     | Instances     |
| :-------------: |:-------------|:-------------:|:-------------:|
| 1    | Auction can be created with zero `totalBaseAmount` | Low | 1 |
| 2    | Named return variables not used anywhere in the functions | NC | 2 |
| 3    | `constant` should be defined rather than using magic numbers  | NC | 4 |


## Findings

### 1- Auction can be created with zero `totalBaseAmount` :

In the `createAuction` function a user can start a new auction without transferring any `baseToken`, this is possible because there are no checks on the values of `auctionParams`, one of them being `totalBaseAmount` which could be set to zero. This will create an auction with zero base tokens and bidders will still be able to bid on it but in the end no fund will be distributed.

#### Risk : Low 

#### Proof of Concept

Instances include:

File: src/SizeSealed.sol [Line 98-100](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L98-L100)
```
SafeTransferLib.safeTransferFrom(
    ERC20(auctionParams.baseToken), msg.sender, address(this), auctionParams.totalBaseAmount
);
```

#### Mitigation

Add a check in the `createAuction` function to verify that the `totalBaseAmount` of the auction is not equal to 0.

### 2- Named return variables not used anywhere in the function :

When Named return variable are declared they should be used inside the function instead of the return statement or if not they should be removed to avoid confusion.

#### Risk : NON CRITICAL

#### Proof of Concept
Instances include:

File: src/SizeSealed.sol

[returns (uint128 tokensAvailable)](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L454)

File: src/util/ECCMath.sol

[returns (bytes32 decryptedMessage)](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L54)

#### Mitigation

Either use the named return variables inplace of the return statement or remove them.

### 3- `constant` should be defined rather than using magic numbers :

It is best practice to use constant variables rather than hex/numeric literal values to make the code easier to understand and maintain, but if they are used those numbers should be well docummented. 

#### Risk : NON CRITICAL

#### Proof of Concept
Instances include:

File: src/SizeSealed.sol

[24 hours](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L35)

[24 hours](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L37)

[24 hours](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L426)

File: src/util/ECCMath.sol

[6000](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L28)

#### Mitigation
Replace the hex/numeric literals aforementioned with `constants`.