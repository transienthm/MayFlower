# 4.2 合约实战-实现标准代币接口

这一节我们来看一下以太坊设置的标准的代币接口ERC-20：[https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md)

有了标准之后，例如钱包，交易所，就可以有一个统一的接口来支持所有的代币。

```
pragma solidity ^0.4.20;

contract ERC20Interface{
    string public name;  // 代币的名字
    string public symbol; // 代币的一个简写
    uint8 public decimals; // 返回代币的小数点位。最基本单位。
    uint public totalSupply; // 总供应量

    function balanceOf(address _owner) view returns (uint256 balance); // 查看余额，必须实现
    function transfer(address _to, uint256 _value) returns (bool success);  // 转账，必须实现
    function transferFrom(address _from, address _to, uint256 _value) returns (bool success); // 从一个地址，转移到另一个地址
    function approve(address _spender, uint256 _value) returns (bool success); // 授权某一个地址可操作指定数量的代币
    function allowance(address _owner, address _spender) view returns (uint256 remaining); // 查看一个地址，可操纵另一个的地址的剩余金额

    event Transfer(address indexed _from, address indexed _to, uint256 _value); // 转账操作触发的事件
    event Approval(address indexed _owner, address indexed _spender, uint256 _value); // 授权操作触发的事件
}
```

下面我们来基于这个接口开发一个标准的代币

```
pragma solidity ^0.4.20;
contract ERC20Interface{
    string public name;
    string public symbol;
    uint8 public decimals;
    uint public totalSupply;
    function transfer(address _to, uint256 _value) returns (bool success);
    function transferFrom(address _from, address _to, uint256 _value) returns (bool success);
    function approve(address _spender, uint256 _value) returns (bool success);
    function allowance(address _owner, address _spender) view returns (uint256 remaining);
    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
}


contract ERC20 is ERC20Interface{
    
    mapping (address => uint256) public balanceOf;
    // 这个映射用来保存一个账户被其他账号授权可操作的代币余额
    mapping (address => mapping (address => uint256)) internal allowed;

    constructor() public{
        name = "GWF FIRST TOKEN";
        symbol = "XIANGSHI";
        decimals = 0;
        totalSupply = 10000;
        balanceOf[msg.sender] = totalSupply;
    }


    function transfer(address _to, uint256 _value) returns (bool success){
        require(_to != address(0));
        require(balanceOf[msg.sender] >= _value);
        require(balanceOf[_to] + _value >= balanceOf[_to]);

        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;

        emit Transfer(msg.sender, _to, _value);
    }

    function transferFrom(address _from, address _to, uint256 _value) returns (bool success){
        require(_to != address(0));
        require(balanceOf[_from] >= _value);
        require(allowed[_from][msg.sender] >= _value);
        require(balanceOf[_to] + _value >= balanceOf[_to]);

        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;

        emit Transfer(msg.sender, _to, _value);

        success = true;
    }

    function approve(address _spender, uint256 _value) returns (bool success){
        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        success = true;
    }
    
    function allowance(address _owner, address _spender) view returns (uint256 remaining){
        return allowed[_owner][_spender];
    }

    event Transfer(address indexed _from, address indexed _to, uint256 _value);

    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
}



```





