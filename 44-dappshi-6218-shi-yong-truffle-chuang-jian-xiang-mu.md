# 4.4 DAPP实战- 使用truffle创建项目

- ### truffle https://truffleframework.com/truffle
   truffle 可以帮助我们创建，编译，测试一个solidity项目。他使用npm进行安装 
```
npm install truffle -g
```

  使用truffle初始化一个工程，可以看到工程下有如下文件
  
```shell
(blockchain_principle-x-Y8P0ch) ~/project/solidity/pet-shop ᐅ truffle init
Downloading...
Unpacking...
Setting up...
Unbox successful. Sweet!

Commands:

  Compile:        truffle compile
  Migrate:        truffle migrate
  Test contracts: truffle test
(blockchain_principle-x-Y8P0ch) ~/project/solidity/pet-shop ᐅ ls
contracts         migrations        test              truffle-config.js truffle.js
(blockchain_principle-x-Y8P0ch) ~/project/solidity/pet-shop ᐅ tree
.
├── contracts
│   └── Migrations.sol -- 合约文件
├── migrations
│   └── 1_initial_migration.js --部署脚本
├── test --测试文件存放目录
├── truffle-config.js --配置文件
└── truffle.js

3 directories, 4 files
(blockchain_principle-x-Y8P0ch) ~/project/solidity/pet-shop ᐅ

```

- ### truffle unbox 
  - box 是 truffle提供的一系列的包（库），例如react等，
  - truffle提供了一些官方的boxs，如pet-shop
  - [unbox](https://truffleframework.com/boxes/pet-shop)命令可以帮我们把官方提供好的box下载好并解压
  
```
(blockchain_principle-x-Y8P0ch) ~/project/solidity/pet-shop_2 ᐅ ls
(blockchain_principle-x-Y8P0ch) ~/project/solidity/pet-shop_2 ᐅ truffle unbox pet-shop
Downloading...
Unpacking...
Setting up...
Unbox successful. Sweet!

Commands:

  Compile:        truffle compile
  Migrate:        truffle migrate
  Test contracts: truffle test
  Run dev server: npm run dev
(blockchain_principle-x-Y8P0ch) ~/project/solidity/pet-shop_2 ᐅ ls
box-img-lg.png    box-img-sm.png    bs-config.json    contracts         migrations        node_modules      package-lock.json package.json      src               test              truffle.js
```

- ### ganache https://truffleframework.com/ganache
ganache 可以是运行在我们本地内存环境的一个虚拟的节点（只有一个节点的区块链），然后会为我们创建账号，trauffle利用这个节点来提供环境测试项目