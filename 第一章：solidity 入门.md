#第一章：solidity 入门

## 一、智能合约

Solidity 的代码都包裹在**合约**里面. 一份`合约`就是以太应币应用的基本模块， 所有的变量和函数都属于一份合约, 它是你所有应用的起点。

```js
pragma solidity ^0.4.20; //声明版本，使用0.4.20
contract HelloWorld{
    //一份空合约
}
```

### 二、状态变量和整数

### 1.状态变量

**状态变量**是被永久地保存在合约中的变量。也就是说它们被写入以太币区块链中。类似成写入一个数据库。

*所以说，传统数据库可以做的事情区块链都能做到。*

```js
contract Example {
  // 这个无符号整数将会永久的被保存在区块链中
  uint myUnsignedInteger = 100;
}
```

### 2.无符号整数：`uint`

`uint` 无符号数据类型， 指**其值不能是负数**，对于有符号的整数存在名为 `int` 的数据类型。

*注: Solidity中， `uint` 实际上是 `uint256`代名词， 一个256位的无符号整数，但是也可以直接定义`unit8`、`uint16`等等。*

## 三、数学运算

与其他语言相同：

- 加法: `x + y`
- 减法: `x - y`,
- 乘法: `x * y`
- 除法: `x / y`
- 取模 / 求余: `x % y` 
- 乘方:`x ** y`

## 四、结构体

即更复杂的数据类型，用多个字段描述1个`srtuct`。

```js
pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;     //用name和dna 两个字段描述一个Zombie
    }

}
```

## 五、数组

Solidity支持`动态数组`和`静态数组`两种数组。

```js
// 固定长度为2的静态数组:
uint[2] fixedArray;
// 固定长度为5的string类型的静态数组:
string[5] stringArray;
// 动态数组，长度不固定，可以动态添加元素:
uint[] dynamicArray;
```

或者也可以创建一个`结构体`的数组，如：

```js
struct Person {
  uint age;
  string name;
}
Person[] people; // dynamic Array, we can keep adding to it
```

*在区块链中，状态变量会被永久保存，所以用动态数组保存结构体数据是非常有必要的。*

#### 公共数组

在Solidity中，可以定义`public`数组，此时回自动创建`getter`方法：

```js
Person[] public people;
```

其它的合约可以从这个数组读取数据（但不能写入数据），所以这在合约中是一个有用的保存公共数据的模式。

# 六、函数

Solidity中定义函数的方法如下：

```js
function eatHamburgers(string _name, uint _amount) {
	//一个空函数，名称为eatHamburgers，接受string类型的name和uint型的amount两个参数。
    //变量名前加“_”是js一个约定俗成的命名方式，加"_"表示私有变量和私有函数，沿用至Solidity。
}
```

# 七、使用结构体和数组

以`Person`为例：

```js
struct Person {
  uint age;
  string name;
}

Person[] public people;
```

创建一个`Person`结构并加入到数组`people`中：

```js
// 创建一个新的Person:
Person wangtongliang = Person(180, "wangtongliang");

// 将新创建的satoshi添加进people数组:
people.push(wangtongliang);

//并作一步：
peolpe.push(Person(180,"wangtongliang"))
```

*array.push()在数组的尾部加入新的元素。*

# 八、私有函数和公有函数

Solidity中函数默认属性为`公共`。意味着其他任何一方（任何其他合约）均可调用该合约内的函数。

使用`private`参数将函数定义为`私有`：

```js
uint[] numbers;
// 使用_表示私有函数。
function _addToArray(uint _number) private {
  numbers.push(_number);
}
```

# 九、函数的更多属性

## 1.返回值

即让一个让一个函数返回一个数值:

```js
// 定义返回值类型为uint
function sayHello() public returns (uint) {
  return 1;
} //该函数并没有改变Solidity里面的状态，即没有改变或写任何东西。
```

## 2.修饰符

即定义函数的性质，常见的函数修饰符如下：

- `pure` : 不允许修改或访问状态变量。
- `view` : 不允许修改状态变量。
- `payable` : 允许函数在调用同时接收Ether。
- `constant` for functions:等同于`view`.

```js
function _generateRandomDna(string _str) private view returns (uint) {
       //一个可见性为private，修饰符为view的函数，返回unit类型的数据。
    }
```

# 十、Keccak256 和 类型转换

## 1.Keccak256：Ethereum内部的一个散列函数

即将一个字符串转化为一个256位的16进制数字，字符串的任意微小变化都会引起散列数据的剧烈变化。

```js
//6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5
keccak256("aaaab");
//b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9
keccak256("aaaac");
```

> 注: 在区块链中**安全地**产生一个随机数是一个很难的问题， 本例的方法不安全，但是在我们的Zombie DNA算法里不是那么重要，已经很好地满足我们的需要了。

## 2.类型转换

跟其他编程语言的数据类型转换基本相同。

# 十一、事件

**事件**是合约和区块链通讯的一种机制。前段应该“监听“事件并作出反应。

```js
// 这里建立事件
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public {
  uint result = _x + _y;
  //触发事件，通知app
  IntegersAdded(_x, _y, result);
  return result;
}
//监听事件并回应
YourContract.IntegersAdded(function(error, result) { 
  // dosomething
}
```

# 十二、Web3.js

以太坊的一个JavaScript库，可以使智能合约与前端进行交互。









