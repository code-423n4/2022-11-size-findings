# 1. Delete storage variable on `SizeSealed.refund` instead of simply setting `sender` to nul for higher gas refunds

Code changes
```diff
diff --git a/src/SizeSealed.sol b/src/SizeSealed.sol
index e5380ff..1b75d05 100644
--- a/src/SizeSealed.sol
+++ b/src/SizeSealed.sol
@@ -344,11 +344,12 @@ contract SizeSealed is ISizeSealed {
             revert InvalidState();
         }
 
-        b.sender = address(0);
+        uint256 quoteAmount = b.quoteAmount;
+        delete a.bids[bidIndex];
 
         emit BidRefund(auctionId, bidIndex);
 
-        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount);
+        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, quoteAmount);
     }
 
     /// @notice Called after finalize for successful bidders
```

Gas diff
```
$ forge snapshot --diff .gas-snapshot
...
testCancelSingleBid() (gas: -12 (-0.002%))
testCancelBidDuringVoidedNoFinalize() (gas: -12 (-0.002%))
testCancelAuction() (gas: -12 (-0.004%))
testCancelAuctionDuringRevealPeriod() (gas: -12 (-0.004%))
testAuctionRefundLostBidder() (gas: -98628 (-10.846%))
Overall gas change: -98676 (-10.859%)
```

# 2.  Delete storage variable on `SizeSealed.`cancelBid` instead of simply setting properties to nul for higher gas refunds

Code changes
```diff
diff --git a/src/SizeSealed.sol b/src/SizeSealed.sol
index e5380ff..e475ba0 100644
--- a/src/SizeSealed.sol
+++ b/src/SizeSealed.sol
@@ -428,15 +428,14 @@ contract SizeSealed is ISizeSealed {
             }
         }
 
+        uint256 quoteAmount = b.quoteAmount;
         // Prevent any futher access to this EncryptedBid
-        b.sender = address(0);
-
         // Prevent seller from finalizing a cancelled bid
-        b.commitment = 0;
+        delete a.bids[bidIndex];
 
         emit BidCancelled(auctionId, bidIndex);
 
-        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, b.quoteAmount);
+        SafeTransferLib.safeTransfer(ERC20(a.params.quoteToken), msg.sender, quoteAmount);
     }
 
     ////////////////////////////////////////////////////////////////////////////

```

Gas diff
```
$ forge snapshot --diff .gas-snapshot
...
testAuctionRefundLostBidder() (gas: -12 (-0.001%))
testCancelBidDuringRevealBeforeFinalize() (gas: -16 (-0.003%))
testCancelAuction() (gas: -12 (-0.004%))
testCancelAuctionDuringRevealPeriod() (gas: -12 (-0.004%))
testCancelBidAfterFinalize() (gas: -32 (-0.005%))
testCancelBidDuringVoidedNoFinalize() (gas: -61462 (-11.399%))
testCancelSingleBid() (gas: -63386 (-11.548%))
Overall gas change: -124932 (-22.964%)
```