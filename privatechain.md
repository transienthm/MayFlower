> 声明，本文档由vim编写

# 一、iterm安装（重要但不想说，因为太简单）

https://juejin.im/post/5a815edd5188251c85636034

# 二、homebrew安装（重要但不想说，因为太简单）

https://www.jianshu.com/p/231f923fb886

# 三、zsh及oh_my_zsh安装（不重要，都懒得说）

略

# 三、geth安装（重要）

**Geth** 又名Go Ethereum. 是以太坊协议的三种实现之一，由Go语言开发，完全开源的项目。Geth 可以被安装在很多操作系统上，包括Windows、Linux、Mac的OSX、Android或者IOS系统

## 3.1 Geth能做什么？

Geth是以太坊协议的具体落地实现，通过Geth，你可以实现以太坊的各种功能,如账户的新建编辑删除，开启挖矿，ether币的转移，智能合约的部署和执行等等

## 3.2 安装

在mac环境下，安装好homebrew，只需要在**iterm**中执行以下两条命令即可

```
brew tap ethereum/ethereum
brew install ethereum
```

安装完成后，在**iterm**中输入geth version，得到以下结果，说明安装成功

```
Geth
Version: 1.8.13-stable
Git Commit: 225171a4bfcc16bd12a1906b1e0d43d0b18c353b
Architecture: amd64
Protocol Versions: [63 62]
Network Id: 1
Go Version: go1.10
Operating System: linux
GOPATH=
GOROOT=/usr/lib/go-1.10
```

**注意：**

1. iterm提示为王童亮同学专属提示，毕竟是产品序列最会开发，开发中最会反爬，反爬中最懂产品的嘛
2. 过程中可能涉及go语言环境搭建，请自行google



# 四、私链的部署

目前私有链只部署在一台机器上，后续按需求扩展机器数量

在云主机某目录下创建一个目录，用于存放私链部署相关文件

```
mkdir privatechain
```

## 4.1 创建创世区块配置文件

```
vim genesis.json
```

输入以下内容

```
{
    "config": {
        "chainId": 0,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "alloc": {
        "0xbdda794a097b41309700fdc5bdba880a7763c738": {
            "balance": "1000"
        }
    },
    "nonce": "0x0000000000000042",
    "difficulty": "0x020000",
    "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "coinbase": "0x0000000000000000000000000000000000000000",
    "timestamp": "0x00",
    "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "extraData": "",
    "gasLimit": "0xffffffff"
}
```

其中：

| 字段       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| alloc      | 用来预置账号以及账号的以太币数量。因为私有链挖矿比较容易，所以不需要预置有币的账号。 |
| nonce      | 生成的工作证明的散列。注意它和 mixhash 的设置需要满足以太坊的 Yellow paper,4.3.4.Block Header Validity,（44）章节所描述的条件。 |
| difficulty | 设置当前区块的难度，如果难度过大，CPU 挖矿就越难，这里设置较小难度，方便 CPU 挖矿，十六进制。 |
| mixhash    | 与 nonce 配合用于挖矿，由上一个区块的一部分生成的 hash。注意它和 nonce 的设置需要满足以太坊的 Yellow paper,4.3.4.Block Header Validity,（44）章节所描述的条件。 |
| coinbase   | 矿工的账号，可以在创建私链之前导入已经创建好的账号。         |
| parentHash | 上一个区块的 hash 值，创始区块没有上一个区块，所以设置为 0。 |
| extraData  | 附加信息，可以填写任意信息，已达到私有链的个性化，可以加入自己的名字等。 |
| gasLimit   | 该值设置对 GAS 的消耗总量限制，用来限制区块能包含的交易信息总和，私有链可以设置为最大值。 |

## 4.2 初始化

先创建data目录

```
mkdir privatechain/data
```

创建数据存放地址并初始化创世块，执行：

```
geth --datadir "privatechain/node" init privatechain/genesis.json
```

## 4.3 启动节点

```
nohup geth --nodiscover --maxpeers 3 --identity "private etherum" --rpc --rpccorsdomain "*" --datadir "/privatechain/data" --port "30303" --rpcport "8546" --rpcapi "db,eth,net,web3" --networkid "20180818" &
```

注意

1. 指定的端口号，在云服务提供商的安全组中需要配置相关接口的出入规则
2. 后台运行使用nohup + & 即可

## 4.4 远程连接

```
geth attach http://ip:8546
```

得到返回

```
geth attach http://ip:8546
Welcome to the Geth JavaScript console!

instance: Geth/v1.8.13-stable-225171a4/linux-amd64/go1.10
 modules: eth:1.0 net:1.0 personal:1.0 rpc:1.0 web3:1.0

>
```

此时已经可以调用eth的api了，例如：

```
personal.newAccount();
Passphrase:
Repeat passphrase:

"0xefed41b6ec7692c510be8f4e87d1ec8f78df23fc"
>
> eth.accounts
["0xefed41b6ec7692c510be8f4e87d1ec8f78df23fc"]
>
```