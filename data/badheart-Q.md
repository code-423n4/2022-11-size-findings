Contest: 2022-11-size
Warden: badheart

QA and Low Issues Report

Overview:
    • Codebase Summary
    • [L006] - Check that Contract Exists before using solmate's SafeTransferLib
        ◦ 9 instances of [L006]

Summary:
	As my first exposure to production Solidity, I do not have much to remark regarding the overall code impressions. I thought it was interesting that a custom ECC encryption mechanism was developed and thought the overall code-base was streamlined and simple (in a good way). Too much complexity can introduce bugs and make the code difficult to understand. This code base, instead, was approachable and even a beginner, like myself, could understand its purpose.

Details:
[L006] - Check that Contract Exists before using solmate's SafeTransferLib
Functions in solmate's SafeTransferLib library do not check whether a token has code at all. This responsibility is delegated to the caller (ie. SizeSealed.sol).
As a call to an address with no code will be a no-op, since low-level calls to non-contracts always return true, a transfer of tokens using solmate's SafeTransferLib will succeed if the token does not have any code.
Therefore, it is recommended to verify that a contract exists before using any SafeTransferLib functions.

Location: SizeSealed.sol :: lines 98 – 100 
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L98

Remedy:
SafeTransferLib.safeTransferFrom(
    require(auctionParams.baseToken.code.length != 0, "Token does not exist"); //Add this to ensure token exists before calling.
    ERC20(auctionParams.baseToken), msg.sender, address(this), auctionParams.totalBaseAmount
);
		

Location: SizeSealed.sol :: line 163
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L163

Remedy:		
SafeTransferLib.safeTransferFrom(
    require(a.params.quoteToken.code.length != 0, "Token does not exist"); //Add this to ensure token exists before calling.
    ERC20(a.params.quoteToken), msg.sender, address(this), quoteAmount
);


 Location: SizeSealed.sol :: line 321
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L321

Remedy:		
SafeTransferLib.safeTransfer(
    require(a.params.baseToken.code.length != 0, "Token does not exist"); //Add this to ensure token exists before calling.
    ERC20(a.params.baseToken), a.data.seller, unsoldBase
);


 Location: SizeSealed.sol :: line 327
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L327

Remedy:
SafeTransferLib.safeTransfer(
    require(a.params.quoteToken.code.length != 0, "Token does not exist"); //Add this to ensure token exists before calling.
    ERC20(a.params.quoteToken), a.data.seller, filledQuote
);		


 Location: SizeSealed.sol :: line 351
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L351

Remedy:
SafeTransferLib.safeTransfer(
    require(a.params.quoteToken.code.length != 0, "Token does not exist"); //Add this to ensure token exists before calling.
    ERC20(a.params.quoteToken), msg.sender, b.quoteAmount
);


 Location: SizeSealed.sol :: line 381
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L381

Remedy:
SafeTransferLib.safeTransfer(
    require(a.params.quoteToken.code.length != 0, "Token does not exist"); //Add this to ensure token exists before calling.
    ERC20(a.params.quoteToken), msg.sender, refundedQuote
);


Location: SizeSealed.sol :: line 384
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L384

Remedy:
SafeTransferLib.safeTransfer(
    require(a.params.baseToken.code.length != 0, "Token does not exist"); //Add this to ensure token exists before calling.
    ERC20(a.params.baseToken), msg.sender, baseTokensAvailable
);

	
Location: SizeSealed.sol :: line 409
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L409

Remedy:
SafeTransferLib.safeTransfer(
    require(a.params.baseToken.code.length != 0, "Token does not exist"); //Add this to ensure token exists before calling.
    ERC20(a.params.baseToken), msg.sender, a.params.totalBaseAmount
);

	
Location: SizeSealed.sol :: line 439
https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol#L439

Remedy:
SafeTransferLib.safeTransfer(
    require(a.params.quoteToken.code.length !=0, "Token does not exist"); //Add this to ensure token exists before calling.
    ERC20(a.params.quoteToken), msg.sender, b.quoteAmount
);
	


