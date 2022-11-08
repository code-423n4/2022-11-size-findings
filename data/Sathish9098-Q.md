1)  use external modifier instead of  public. It provides performance benefits

File: 2022-11-size/src/SizeSealed.sol

  
function computeCommitment(bytes32 message) public pure returns (bytes32) {
        return keccak256(abi.encode(message));
    }