# 3.2 solidity 类型讲解

[https://solidity.readthedocs.io/en/v0.4.23/types.html](https://solidity.readthedocs.io/en/v0.4.23/types.html)

solidity 是静态语言，他的类型分为两类：值类型和引用类型

* ### 值类型

  ![image.png](https://upload-images.jianshu.io/upload_images/7220971-f4c0b6b93b5797f1.png?imageMogr2/auto-orient/strip|imageView2/2/w/1240)

  * 其中整型包括uint8，uint32，uint64，uint256，uint 五种类型，代表不同的位数，uint和uint256 都是256位
  * 在c和c++里面，字符串常量通常最后都会有个\0结尾符，在solidity中没有
  * 十六进制常量以hex开头，如`hex"abcd"`，并且16进制常量可以转换成字节和字节数组，每两位为一个字节
    ```solidity
    function testHexLiterals() public constant returns(bytes2,bytes1,bytes1){
      bytes2 a = hex"abcd"
      return [a,a[0],a[1]]
    }
    ```
  * 地址类型 address 表示一个账户地址（20字节）如`Ox72ba7d8e73fe8eb666ea66ba bc8116a41 bfb1Oe2` ，他最重要的两个成员是属性banlance标示一个账户的余额和transfer\(\) 方法用于转账操作。
  * 验证一个合法定制的方法：[https://github.com/ethereum/EIPs/blob/master/EIPS/eip-55.md](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-55.md)

* ### 引用类型

  * 数据位置（Data location）

    引用类型比较复杂，因此在使用的时候，我们要考虑到占用空间的问题。通常他的空间要占用到256位。在拷贝的时候，开销很大。因此我们在拷贝的时候要考虑到数据存储位置的问题。一般有两种存储位置storage（永久存储在区块链中）和 memory（存储在函数调用的临时内存空间中）
    数据的存储和计算都是需要花费gas的，storage的开销要远大于memory。
  * 数组类型（Arrays）
  ![image.png](https://upload-images.jianshu.io/upload_images/7220971-20a1c9a7a1f475ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    数组一般有两个成员： 属性length和push() 像数组中添加新元素
  




