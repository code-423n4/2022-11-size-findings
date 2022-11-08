<h1> INAPPROPRIATE COMMENT <\h1>

what do they mean by tax tokens ?

(Non-critical 1 ) i attached the permalink of it

https://github.com/code-423n4/2022-11-size/blob/706a77e585d0852eae6ba0dca73dc73eb37f8fb6/src/SizeSealed.sol#L95

(non-critical 2)  Inappropriate comment or logic of 'if condition' needs to change

if we follow according to the logic of 'if condition'

then we need to change the comment to Max of 999 bids

else

if the comment is correct then we need to change the if condition to
if (bidIndex >1000) {   (or)   if (bidIndex >= 1001){

here is the permalink of it

https://github.com/code-423n4/2022-11-size/blob/79aa9c01987e57a760521acecfe81b28eab3b313/src/SizeSealed.sol#L156-L157