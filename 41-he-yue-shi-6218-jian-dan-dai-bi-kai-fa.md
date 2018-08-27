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

这就是一个简单的代币，拥有转账和查询余额的功能。但是这个代码是有一些小问题的，他是没有名字的，并且也不知道他的总供应量是多少等等。

以太坊为了规范代币合约的编写，提出了一个代币合约的编写叫erc-20.