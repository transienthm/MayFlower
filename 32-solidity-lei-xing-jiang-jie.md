# 3.2 solidity 类型讲解

https://solidity.readthedocs.io/en/v0.4.23/types.html

solidity 是静态语言，他的类型分为两类：值类型和引用类型

- ### 值类型
![image.png](https://upload-images.jianshu.io/upload_images/7220971-f4c0b6b93b5797f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    - 其中整型包括uint8，uint32，uint64，uint256，uint 五种类型，代表不同的位数，uint和uint256 都是256位
    - 在c和c++里面，字符串常量通常最后都会有个\0结尾符，在solidity中没有
    - 十六进制常量以hex开头，如```hex"abcd"```，并且16进制常量可以转换成字节和字节数组，每两位为一个字节
    ```solidity
    function testHexLiterals() public constant returns(bytes2,bytes1,bytes1){
        bytes2 a = hex"abcd"
        return [a,a[0],a[1]]
    }
    ```
    
    