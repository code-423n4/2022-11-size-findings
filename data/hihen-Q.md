- Constants should be defined rather than using magic numbers. It is bad practice to use numbers directly in code without explanation.
  - All `type(uint128).max`, `type(uint32).max` and `24 hours` in [SizeSealed.sol](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol)
  - [SizeSealed.sol#L157](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L157): `1000`

- Missing validity check for important parameters
  - [SizeSeal.sol: createAuction()](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L55-L59): `auctionParams.quoteToken`, `auctionParams.pubKey`, `encryptedSellerPrivKey`
  - [SizeSealed.sol: bid()](https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L122-L130): `commitment`, `pubKey`, `encryptedMessage`, `encryptedPrivateKey` must not be zero or empty. This can significantly increase the cost of invalid trade attacks (DOS).
