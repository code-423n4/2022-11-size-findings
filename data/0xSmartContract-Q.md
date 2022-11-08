### [N-01] Solidity compiler optimizations can be problematic


```js
foundry.toml:
  1: [profile.default]
  2: src = 'src'
  3: out = 'out'
  4: libs = ['lib']
  5: 
  6: optimizer=true
  7  via_ir=true

```

**Description:**
Protocol has enabled optional compiler optimizations in Solidity.
There have been several optimization bugs with security implications. Moreover, optimizations are actively being developed. Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. 

Therefore, it is unclear how well they are being tested and exercised.
High-severity security issues due to optimization bugs have occurred in the past. A high-severity bug in the emscripten-generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. 

Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. More recently, another bug due to the incorrect caching of keccak256 was reported.
A compiler audit of Solidity from November 2018 concluded that the optional optimizations may not be safe.
It is likely that there are latent bugs related to optimization and that new bugs will be introduced due to future optimizations.

Exploit Scenario
A latent or future bug in Solidity compiler optimizations—or in the Emscripten transpilation to solc-js—causes a security vulnerability in the contracts.



**Recommendation:**
Short term, measure the gas savings from optimizations and carefully weigh them against the possibility of an optimization-related bug.
Long term, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.


### [N-02] NatSpec is missing 

**Description:**
NatSpec is missing for the following functions, mappings , structs and modifiers:

**Context:**
[SizeSealed.sol#L20](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L20)
[SizeSealed.sol#L22](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L22)
[SizeSealed.sol#L28](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L28)
[SizeSealed.sol#L466](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L466)
[SizeSealed.sol#L470](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L470)
[SizeSealed.sol#L474](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L474)
[SizeSealed.sol#L478](https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L478)
[ECCMath.sol#L11](https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L11)


### [N-03] For functions, follow Solidity standard naming conventions

**Context:**

```solidity
src/util/CommonTokenMath.sol:
  47:     function tokensAvailableAtTime(
  48:         uint32 vestingStart,
  49:         uint32 vestingEnd,
  50:         uint32 currentTime,
  51:         uint128 cliffPercent,
  52:         uint128 baseAmount
  53:     ) internal pure returns (uint128) {
  54          if (currentTime > vestingEnd) {


src/util/ECCMath.sol:
  17      /// @dev calculates G^k, aka G^privateKey = publicKey
  18:     function publicKey(uint256 privateKey) internal view returns (Point memory) {


src/util/ECCMath.sol:
  24      /// @return corresponding point
  25:     function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {

src/util/ECCMath.sol:
  36      /// @param message arbitrary 32 bytes
  37:     function encryptMessage(Point memory encryptToPub, uint256 encryptWithPriv, bytes32 message)
  38:         internal
  39:         view
  40          returns (Point memory buyerPub, bytes32 encryptedMessage)

```

**Description:**
The above codes don't follow Solidity's standard naming convention,
internal and private functions : the mixedCase format starting with an underscore (_mixedCase starting with an underscore)
public and external functions : only mixedCase
constant variable : UPPER_CASE_WITH_UNDERSCORES (separated by uppercase and underscore)

### [N-04] Event is missing indexed fields

**Description:**
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

**Context:**

```solidity
src/interfaces/ISizeSealed.sol:

   97:     event AuctionCreated(
   98:         uint256 auctionId, address seller, AuctionParameters params, Timings timings, bytes encryptedPrivKey
   99:     );

  102: 
  103:     event Bid(
  104:         address sender,
  105:         uint256 auctionId,
  106:         uint256 bidIndex,
  107:         uint128 quoteAmount,
  108:         bytes32 commitment,
  109:         ECCMath.Point pubKey,
  110:         bytes32 encryptedMessage,
  111:         bytes encryptedPrivateKey
  112:     );
```
