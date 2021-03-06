# 3.3 全局变量和函数

全局变量和函数可以理解为solidity提供的一些API，在我们编写智能合约的时候，我们可以使用这些变量和函数去获得一些属性，他主要分为以下四类：

1. ### 有关区块和交易
    常用的区块和交易相关的函数和属性有：
    - msg.sender (address) ：获取交易者发送的地址
    - msg.value (uint)：当前交易所附带的以太币，单位是wei
    - block.coinbase (address)：当前旷工的地址
    - block.difficulty (uint)：当前块的难度
    - block.number (uint)：当前区块的块号，索引
    - block.timestamp (uint)：当前块的unix时间戳
    - now (uint)：当前区块的时间戳，timestamp的别名
    - tx.gasprice (uint)：当前交易的gas
2. ### 有关错误处理
    - 什么是错误处理：指在程序发生错误时的处理方式
    - 处理方式：回退状态，solidity没有try-catch语句，当一个错误发生的时候的时候，会回退所有的状态变化，就像所有的错误都没有发生过一样。但是消耗的gas就是消耗了。
    - 为什么这么处理呢：我们可以把区块链当成分布式的事务性的数据库。即每一个调用都是事务性的，要么全部成功，要么全部失败
    - 错误如何来被处理：
        - assert（内） 通常是用来检测函数内部的错误；会抛出Assert异常，会消耗掉所有的gas
        - require（外）  通常是用来检查输入的变量，或者是合约的状态变量是否满足条件，Require不会
    ```solidity
        function sendHalf(address addr) public payable returns(uint balance){
        require(msg.value % 2 == 0);
        
        uint balanceBeforeTransfer = this.balance;
        addr.transfer(msg.value / 2 + 1);
         
        assert(this.balance == balanceBeforeTransfer / 2);
        
        return this.balance;
    }
    ```
3. ### 有关数字和加密功能
4. ### 有关地址和合约



