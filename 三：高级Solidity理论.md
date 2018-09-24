# 三、高级Solidity理论

## 1.智能合约的永固性

在把智能合约上传到以太坊后，他就变得**不可更改**，会被永久的存在以太坊的网络上，如果你的智能合约代码有任何漏洞，在上传后也不能进行补救。

## 2.Ownable Contracts

创建一个`Ownable`合约：

- 首先创造一个构造函数，将`owner`设置为`msg.sender`	
- 加上一个修饰符`onlyOwner`，将某些函数的权限锁定在`owner`上。
- 允许将合约的所有权转让给他人。

```js
/**
 * @title Ownable
 * @dev The Ownable contract has an owner address, and provides basic authorization control
 * functions, this simplifies the implementation of "user permissions".
 */
contract Ownable {
  address public owner;
  event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

  /**
   * @dev The Ownable constructor sets the original `owner` of the contract to the sender
   * account.
   */
  function Ownable() public {
    owner = msg.sender;
  }

  /**
   * @dev Throws if called by any account other than the owner.
   */
  modifier onlyOwner() {
    require(msg.sender == owner);
    _;
  }

  /**
   * @dev Allows the current owner to transfer control of the contract to a newOwner.
   * @param newOwner The address to transfer ownership to.
   */
  function transferOwnership(address newOwner) public onlyOwner {
    require(newOwner != address(0));
    OwnershipTransferred(owner, newOwner);
    owner = newOwner;
  }
}
```

- 构造函数：`function Ownable()`是一个 **constructor** (构造函数)，构造函数不是必须的，它与合约同名，构造函数一生中唯一的一次执行，就是在合约最初被创建的时候。

## 3.函数修饰符

函数修饰符看起来跟函数没什么不同，不过关键字`modifier` 告诉编译器，这是个`modifier(修饰符)`，而不是个`function(函数)`。它不能像函数那样被直接调用，只能被添加到函数定义的末尾，用以改变函数的行为。

一个函数修饰符：

```js
/**
 * @dev 调用者不是‘主人’，就会抛出异常
 */
modifier onlyOwner() {
  require(msg.sender == owner);
  _;
}
```

使用函数修饰符：

```js
contract MyContract is Ownable {
  event LaughManiacally(string laughter);

  // `onlyOwner`的使用 :
  function likeABoss() external onlyOwner {
    LaughManiacally("Muahahahaha");
  }
}
```

当调用函数`likeABoss`时，首先执行`onlyOwner`中的代码，执行到`onlyOwner`的`_;`时，程序再返回并执行`likeABoss`的代码。

函数修饰符可以快速应用到多种场合，但是最常用的还是在函数之前快速添加`require`检查。

## 4.Gas驱动的编程

​	在 Solidity 中，你的用户想要每次执行你的 DApp 都需要支付一定的 **gas**，gas 可以用以太币购买，因此，用户每次跑 DApp 都得花费以太币。

​	一个 DApp 收取多少 gas 取决于功能逻辑的复杂程度。每个操作背后，都在计算完成这个操作所需要的计算资源，（比如，存储数据就比做个加法运算贵得多）， 一次操作所需要花费的 **gas** 等于这个操作背后的所有运算花销的总和。

​	所以Solidity编程比起其他编程语言更强调优化。

### 为什么需要Gas驱动？

以太坊就像一个巨大、缓慢、但非常安全的电脑。当你运行一个程序的时候，网络上的每一个节点都在进行相同的运算，以验证它的输出 —— 这就是所谓的”去中心化“ 由于数以千计的节点同时在验证着每个功能的运行，这可以确保它的数据不会被被监控，或者被刻意修改。

*所以，为了防止有用户用无限循环或密集计算占用大量资源，所以在以太坊上运算或者存储，需要付费。*

### 如何省Gas：结构封装 （Struct packing）

通常情况下我们不会考虑使用 `uint` 变种，因为无论如何定义 `uint`的大小，Solidity 为它保留256位的存储空间。例如，使用 `uint8` 而不是`uint`（`uint256`）不会为你节省任何 gas。

**除非把uint绑定到`struct`里面。**

如果一个 `struct` 中有多个 `uint`，则尽可能使用较小的 `uint`，Solidity 会将这些 `uint` 打包在一起，从而占用较少的存储空间。

当`uint`定义在一个`struct` 中的时候：

- 尽量使用最小的整数子类型以节约空间。
- 把同样类型的变量放在一起（即在 struct 中将把变量按照类型依次放置）。

如：

```
uint c; uint32 a; uint32 b;` 和 `uint32 a; uint c; uint32 b;
```

前者比后者需要的gas更少，因为前者把`uint32`放一起了。

## 5.Solidity的时间单位

Solidity 使用自己的本地时间单位。

变量 `now` 将返回当前的unix时间戳（自1970年1月1日以来经过的秒数）。我写这句话时 unix 时间是 `153771628`。

> 注意：Unix时间传统用一个32位的整数进行存储。这会导致“2038年”问题，当这个32位的unix时间戳不够用，产生溢出，使用这个时间的遗留系统就麻烦了。所以，如果我们想让我们的 DApp 跑够20年，我们可以使用64位整数表示时间，但为此我们的用户又得支付更多的 gas。emmm.....

Solidity 还包含`秒(seconds)`，`分钟(minutes)`，`小时(hours)`，`天(days)`，`周(weeks)` 和 `年(years)` 等时间单位。它们都会转换成对应的秒数放入 `uint` 中。所以 `1分钟` 就是 `60`，`1小时`是 `3600`（60秒×60分钟），`1天`是`86400`（24小时×60分钟×60秒），以此类推。

```js
uint lastUpdated;

// 将‘上次更新时间’ 设置为 ‘现在’
function updateTimestamp() public {
  lastUpdated = now;
}

// 如果到上次`updateTimestamp` 超过5分钟，返回 'true'
// 不到5分钟返回 'false'
function fiveMinutesHavePassed() public view returns (bool) {
  return (now >= (lastUpdated + 5 minutes));
}
```

## 6.将结构体作为参数传入

由于结构体的存储指针可以以参数的方式传递给一个 `private` 或 `internal` 的函数，因此结构体可以在多个函数之间相互传递。

遵循这样的语法：

```js
struct Zombie {
      string name;
      uint dna;
      uint32 level;
      uint32 readyTime;
    }

function _doStuff(Zombie storage _zombie) internal {
  _zombie.name = "wangbin250";
}
```

## 7.公有函数和安全性

你必须仔细地检查所有声明为 `public` 和 `external`的函数，一个个排除用户滥用它们的可能，谨防安全漏洞。请记住，如果这些函数没有类似 `onlyOwner` 这样的函数修饰符，用户能利用各种可能的参数去调用它们。

## 8.带参数的函数修饰符

除之前已经涉及到的函数修饰符`onlyOwner`，函数修饰符也可以带参数：

```js
// 存储用户年龄的映射
mapping (uint => uint) public age;

// 限定用户年龄的修饰符
modifier olderThan(uint _age, uint _userId) {
  require(age[_userId] >= _age);
  _;
}

// 必须年满18周岁才允许开车 
// 我们可以用如下参数调用 olderThan 修饰符:
function driveCar(uint _userId) public olderThan(18, _userId) {
  // drive
}
```

## 9.利用`View`函数节省Gas

当用户从外部调用一个`view`函数时，时不需要支付gas的。

这是因为 `view` 函数不会真正改变区块链上的任何数据 - 它们只是读取。因此用 `view` 标记一个函数，意味着告诉 `web3.js`，运行这个函数只需要查询你的本地以太坊节点，而不需要在区块链上创建一个事务（事务需要运行在每个节点上，因此花费 gas）。

> 注意：如果一个 `view` 函数在另一个函数的内部被调用，而调用函数与 `view` 函数的不属于同一个合约，也会产生调用成本。这是因为如果主调函数在以太坊创建了一个事务，它仍然需要逐个节点去验证。所以标记为 `view` 的函数只有在外部调用时才是免费的。

*so，使用`external view`作为修饰符的函数一定不会花费gas。*

## 10.如何规避昂贵的存储成本

Solidity 使用`storage`(存储)是相当昂贵的，”写入“操作尤其贵。

这是因为，无论是写入还是更改一段数据， 这都将永久性地写入区块链。需要在全球数千个节点的硬盘上存入这些数据，随着区块链的增长，拷贝份数更多，存储量也就越大。

为了降低成本，不到万不得已，避免将数据写入存储。这也会导致效率低下的编程逻辑 - 比如每次调用一个函数，都需要在 `memory`(内存) 中重建一个数组，而不是简单地将上次计算的数组给存储下来以便快速查找。

在大多数编程语言中，遍历大数据集合都是昂贵的。但是在 Solidity 中，使用一个标记了`external view`的函数，遍历比 `storage` 要便宜太多，因为 `view` 函数不会产生任何花销。

### 在内存中声明数组：

在数组后面加上 `memory`关键字， 表明这个数组是仅仅在内存中创建，不需要写入外部存储，并且在函数调用结束时它就解散了。与在程序结束时把数据保存进 `storage` 的做法相比，内存运算可以大大节省gas开销 -- 把这数组放在`view`里用，完全不用花钱。

demo:

```js
function getArray() external pure returns(uint[]) {
  // 初始化一个长度为3的内存数组
  uint[] memory values = new uint[](3);
  // 赋值
  values.push(1);
  values.push(2);
  values.push(3);
  // 返回数组
  return values;
}
```

> 内存数组 **必须** 用长度参数（在本例中为`3`）创建。目前不支持 `array.push()`之类的方法调整数组大小，在未来的版本可能会支持长度修改。

## 11.Solidity中的for循环

`for`循环的语法在 Solidity 和 JavaScript 中类似。

来看一个创建偶数数组的例子：

```
function getEvens() pure external returns(uint[]) {
  uint[] memory evens = new uint[](5);
  // 在新数组中记录序列号
  uint counter = 0;
  // 在循环从1迭代到10：
  for (uint i = 1; i <= 10; i++) {
    // 如果 `i` 是偶数...
    if (i % 2 == 0) {
      // 把它加入偶数数组
      evens[counter] = i;
      //索引加一， 指向下一个空的‘even’
      counter++;
    }
  }
  return evens;
}
```

这个函数将返回一个形为 `[2,4,6,8,10]` 的数组。

*比python的for 和 if 麻烦好多哦。*

