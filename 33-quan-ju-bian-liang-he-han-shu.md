# 3.3 全局变量和函数

全局变量和函数可以理解为solidity提供的一些API，在我们编写智能合约的时候，我们可以使用这些变量和函数去获得一些属性，他主要分为以下四类：

1. ### 有关区块和交易
    常用的区块和交易相关的函数和属性有：
    - msg.sender (address) ：获取交易者发送的地址
    - msg.value (uint)：当前交易所附带的以太币，单位是wei
    - block.coinbase (address)：当前块的地址
    - block.difficulty (uint)：当前块的难度
    - block.number (uint)：当前区块的块号，索引
    - block.timestamp (uint)：当前块的unix时间戳
    - now (uint)：当前区块的时间戳，timestamp的别名
    - tx.gasprice (uint)：当前交易的gas
2. ### 有关错误处理
3. ### 有关数字和加密功能
4. ### 有关地址和合约



