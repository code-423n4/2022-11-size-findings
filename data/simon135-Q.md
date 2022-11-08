### this if statment might not  work because of the wrong check 
```
            // Fill the remaining unfilled base amount
            if (data.filledBase + baseAmount > data.totalBaseAmount) {
                //@done  this if statement is wrong
                baseAmount = data.totalBaseAmount - data.filledBase;
            }

            b.filledBaseAmount = baseAmount;
            data.filledBase += baseAmount;
        
```
if `baseAmount`  is  more then `totalBaseAmonut`  then it will revert 
```
   // Auction has been fully filled
            if (data.filledBase == data.totalBaseAmount) continue;

```
https://github.com/code-423n4/2022-11-size/blob/969b9591b89ab21dcc9a13925809696dcaf43938/src/SizeSealed.sol#L289