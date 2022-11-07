## LACK OF REENTRANCY GUARDS

Whenever SafeTransferLib.safeTransfer() is called, the to address can re-enter back to the contracts due to the ERC1155TokenReceiver(to).onERC1155Received(msg.sender, from, id, amount, data) code (hook)










