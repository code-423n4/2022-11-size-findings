### [L-01] ```approve()``` and ```safeApprove()``` should be replaced with ```safeIncreaseAllowance()``` / ```safeDecreaseAllowance()```


#### Impact
```approve()``` & ```safeApprove()``` are deprecated and subject to a known front-running attack. Consider using  ```safeIncreaseAllowance()``` & ```safeDecreaseAllowance()``` instead.


#### Findings:
```
2022-11-size/src/test/mocks/MockSeller.sol:L111                 baseToken.approve(address(auctionContract), type(uint256).max);

2022-11-size/src/test/mocks/MockBuyer.sol:L119                 quoteToken.approve(address(auctionContract), type(uint256).max);

```

### [L-02] ```require()```/```revert()``` statements should have descriptive strings.


#### Impact
Consider adding descriptive strings in ```require()```/```revert()```.


#### Findings:
```
2022-11-size/src/test/mocks/MockBuyer.sol:L44        require(quoteToken.balanceOf(address(this)) >= quoteAmount);

2022-11-size/src/test/mocks/MockBuyer.sol:L81        require(quoteToken.balanceOf(address(this)) >= quoteAmount);

```


### [L-03] ```_safemint()``` should be used rather than ```_mint()``` wherever possible


#### Impact
```_mint()``` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of ```_safeMint()``` which ensures that the recipient is either an EOA or implements ```IERC721Receiver```. Both [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/transmissions11/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function


#### Findings:
```
2022-11-size/src/test/mocks/MockERC20.sol:L10        _mint(to, amount);

```
