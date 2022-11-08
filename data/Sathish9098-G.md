1)  IF THE OPERATOR IS "||" NO NEED TO CHECK MULTIPLE CONDITIONS. IF FIRST CONDITION IS TRUE THEN CAN SKIP REMAINING CONDITION CHECKS. IF CONDITION IS FALSE THEN ONLY NEED TO CHECK NEXT NEXT CONDITIONS. 

Saves at least 17 gas per skipped conditions 

There are 4 instances of this issue:

File : 2022-11-size/src/util/ECCMath.sol

27 :           if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);

File : 2022-11-size/src/SizeSealed.sol

144:          if (quoteAmount == 0 || quoteAmount == type(uint128).max || quoteAmount < a.params.minimumBidQuote) {

187:          if (pubKey.x != a.params.pubKey.x || pubKey.y != a.params.pubKey.y || (pubKey.x == 1 && pubKey.y == 1)) {

426:          if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {

    
-------------------------------------------------------------------------------------------------------------------------------

2) ++i COSTS LESS GAS THAN i++, ESPECIALLY WHEN IT’S USED IN FOR-LOOPS (--i/i-- TOO)

File : 2022-11-size/src/SizeSealed.sol

302:          for (uint256 i; i < bidIndices.length; i++) {

244:          for (uint256 i; i < seenBidMap.length - 1; i++) {

----------------------------------------------------------------------------------------------------------------------------------------

3)  FOR "&&" OPERATORS IF FIRST CONDITION FALSE THEN NO NEED TO CHECK REMAINING CONDITIONS. && OPERATOR IF ANY ONE CONDITION FALSE THEN OVER ALL CONDITIONS BECOME FALSE. NEED TO CHECK NEXT NEXT CONDTIONS ONLY AFTER PREVIOUS CONDITION IS TRUE 

File : 2022-11-size/src/SizeSealed.sol

258:          if (sharedPoint.x == 1 && sharedPoint.y == 1) continue;


--------------------------------------------------------------------------------------------------------------------------------------------

4)   INSTEAD OF += OPERATOR CAN USE NORMAL ADDITION OPERATORE LIKE data.filledBase=data.filledBase+ baseAmount .
+= OPERATOR CONSUMES MORE GAS FEE COMPARE TO NORMAL + OPERATOR 


File : 2022-11-size/src/SizeSealed.sol

294:    data.filledBase += baseAmount;



---------------------------------------------------------------------------------------------------------------------

5)   