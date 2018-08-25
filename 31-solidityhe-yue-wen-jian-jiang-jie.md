# 3.1 solidity合约文件讲解

solidity 工具箱：http://liyuechun.org/2017/08/16/solidity-001/ 

以下是Solidity的文件结构

![image.png](https://upload-images.jianshu.io/upload_images/7220971-f2dfa7a2fced831b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```solidity
// 声明版本
pragma solidity ^0.4.0;

// import
import "somo_sol_to_import.sol"

// This is a Contract
// 合约
contract Test{
    // 状态变量
    uint a;

    // 函数
    function setA(uint x) public {
        a = x;
        emit Set_A(a);
    }

    // 事件
    event Set_A(uint a);
   
    // 结构类型
    struct Position{
        int lat;
        int lng;
    }

    // 函数修改器
    address public ownerAddr; 

    modifier owner(){
        require(msg.sender == owerAddr);
        _;
    }

    function mine() public owner {
        
    }

}

```