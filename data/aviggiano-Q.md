# 1. Define magic numbers as constant values in order to improve the code's legibility

a) `MAX_BIDS`

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
+    uint256 private constant MAX_BIDS = 1000;
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

b) `REVEAL_PERIOD`

```diff
diff --git a/src/SizeSealed.sol b/src/SizeSealed.sol
index e5380ff..961123c 100644
--- a/src/SizeSealed.sol
+++ b/src/SizeSealed.sol
@@ -13,6 +13,8 @@ import {CommonTokenMath} from "./util/CommonTokenMath.sol";
 /// @title Size Sealed Auction
 /// @author Size Market
 contract SizeSealed is ISizeSealed {
+    uint256 private constant REVEAL_PERIOD = 24 hours;
+
     ///////////////////////////////
     ///          STATE          ///
     ///////////////////////////////
@@ -32,9 +34,9 @@ contract SizeSealed is ISizeSealed {
             if (_state != States.AcceptingBids) revert InvalidState();
         } else if (a.data.lowestQuote != type(uint128).max) {
             if (_state != States.Finalized) revert InvalidState();
-        } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
+        } else if (block.timestamp <= a.timings.endTimestamp + REVEAL_PERIOD) {
             if (_state != States.RevealPeriod) revert InvalidState();
-        } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
+        } else if (block.timestamp > a.timings.endTimestamp + REVEAL_PERIOD) {
             if (_state != States.Voided) revert InvalidState();
         } else {
             revert();
@@ -423,7 +425,7 @@ contract SizeSealed is ISizeSealed {
 
         // Only allow bid cancellations while not finalized or in the reveal period
         if (block.timestamp >= a.timings.endTimestamp) {
-            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {
+            if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + REVEAL_PERIOD) {
                 revert InvalidState();
             }
         }

```
