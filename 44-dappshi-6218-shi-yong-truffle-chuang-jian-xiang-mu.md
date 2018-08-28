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
│   └── Migrations.sol
├── migrations
│   └── 1_initial_migration.js
├── test
├── truffle-config.js
└── truffle.js

3 directories, 4 files
(blockchain_principle-x-Y8P0ch) ~/project/solidity/pet-shop ᐅ

```