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



