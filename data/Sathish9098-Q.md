ECCMath.sol  #L27 

 In IF statement we don't want to check both conditions. here using "||" operator so if first CONDITION true then we can skip second condition. Second condition only checked when the first condition is false. In this way can save gas fee 


/// @notice calculates point^scalar
    /// @dev returns (1,1) if the ecMul failed or invalid parameters
    /// @return corresponding point
    function ecMul(Point memory point, uint256 scalar) internal view returns (Point memory) {
        bytes memory data = abi.encode(point, scalar);
        if (scalar == 0 || (point.x == 0 && point.y == 0)) return Point(1, 1);
        (bool res, bytes memory ret) = address(0x07).staticcall{gas: 6000}(data);
        if (!res) return Point(1, 1);
        return abi.decode(ret, (Point));
    }

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol#L27

SOLUTIONS : 

NEED TO RESTRUCTURE THE IF STATEMENT. THE SECOND CONDITION ONLY CHECKED ONLY WHEN FIRST CONDITION IS false 

