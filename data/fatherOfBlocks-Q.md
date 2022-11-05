**SizeSealed**
- L29-41 - A more readable way to perform this type of validation is with a switch case.
In addition to this

- L61/64/67/70/73 - All createAuction() validations return the same Error, perhaps it would be more appropriate to create one for each case, since it would provide more correct information to users.


**ISizeSealed**
- L18 - The CommitmentMismatch() error is created and is never used, this is something that should be removed as it takes up unnecessary space in the contract.


**ECCMath**
- L51 - The decryptMessage() function is created and is never used, this is something that should be removed as it takes up unnecessary space in the contract.
