**SizeSealed.sol - lines 28 to 43**
atState never called with _state equal to States.Created or States.Voided
previous code may be kept in comment, for future evolutions
gas can be saved with modifications as follows :
    
   ``` 
modifier atState(Auction storage a, States _state) {
        if (block.timestamp < a.timings.startTimestamp) {
            // if (_state != States.Created) revert InvalidState();
            revert InvalidState();
        } else if (block.timestamp < a.timings.endTimestamp) {
            if (_state != States.AcceptingBids) revert InvalidState();
        } else if (a.data.lowestQuote != type(uint128).max) {
            if (_state != States.Finalized) revert InvalidState();
        } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {
            if (_state != States.RevealPeriod) revert InvalidState();
     // } else if (block.timestamp > a.timings.endTimestamp + 24 hours) {
     //     if (_state != States.Voided) revert InvalidState();
        } else {
     //     revert ();
            revert InvalidState();
        }
        _;
    }
```

**SizeSealed.sol - lines 156 to 159**
To save gas on complicated calculations, following test can be made earlier, 
on second line of this function, between lines 131 and 132
```
        // Max of 1000 bids on an auction to prevent DOS
        if (bidIndex >= 1000) {
            revert InvalidState();
        }
```
