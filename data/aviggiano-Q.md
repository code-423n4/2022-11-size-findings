# 1. Define magic numbers as constant values in order to improve the legibility of the code

```diff
diff --git a/src/SizeSealed.sol b/src/SizeSealed.sol
index e5380ff..c43001b 100644
--- a/src/SizeSealed.sol
+++ b/src/SizeSealed.sol
@@ -13,6 +13,9 @@ import {CommonTokenMath} from "./util/CommonTokenMath.sol";
 /// @title Size Sealed Auction
 /// @author Size Market
 contract SizeSealed is ISizeSealed {
+    // Max of 1000 bids on an auction to prevent DOS
+    uint256 constant MAX_BIDS = 1000;
+
     ///////////////////////////////
     ///          STATE          ///
     ///////////////////////////////
@@ -153,8 +156,7 @@ contract SizeSealed is ISizeSealed {
         ebid.encryptedMessage = encryptedMessage;
 
         uint256 bidIndex = a.bids.length;
-        // Max of 1000 bids on an auction to prevent DOS
-        if (bidIndex >= 1000) {
+        if (bidIndex >= MAX_BIDS) {
             revert InvalidState();
         }
```