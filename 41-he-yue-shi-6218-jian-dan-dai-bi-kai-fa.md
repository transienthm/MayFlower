# 4.1 合约实战-简单代币开发

```solidity
pragma solidity ^0.4.20;

contract MyToken{
    mapping(address => uint256) public balance0f;
    
    constructor (uint256 initSupply) public {
        balance0f[msg.sender] = initSupply;
    }
    
    function transfer(address _to,uint _value) public{
        require(balance0f[msg.sender] >= _value);
        require(balance0f[_to] + _value >= balance0f[_to]);
        
        balance0f[msg.sender] -= _value;
        balance0f[_to] += _value;
    }
}
```