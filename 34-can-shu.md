# 3.4 solidity 参数和控制结构

* #### 输入参数
这个很简单了，就是函数的参数列表穿的参数
* #### 输出参数
在函数定义的returns部分可以声明输出参数，然后在函数体对其进行赋值，这时候不需要return
```solidity
    function testSimpleOutPut(uint a,uint b) public constant returns(uint sum,uint mul){
        sum = a + b;
        mul = a * b;
    }
```
* #### 命名参数
在调用函数的时候可以不按照顺序，而是使用{}将参数名和要传的值以key:value方式用,进行拼接。
```solidity
    function testNameParameter() public constant{
        testSimpleOutPutParameter({b:1,a:2});
    }
```
* #### 解构参数
当函数有多个返回值的时候，调用函数的时候需要结构来获取多个返回值
```solidity
    function f() public constant returns (uint a,string b,uint c){
        return (1,"dsfs",2);
    }
    
    function testf() public constant {
        (x,y,z) = f();
        (_,y,z) = f();
        (y,_,z) = f();
    }
```

* #### 控制结构
solidity 的控制结构有 ```if, else, while, do, for, break, continue, return, ? :``` 没有 ```switch,goto```