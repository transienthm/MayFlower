# 4.5 DAPP实战-宠物领养合约编写

- ### 开发
在上一节我们创建的pet-shop/contracts 目录下，来编写我们领养宠物的只能合约，这个合约很简单,只需要完成记录宠物领养者和宠物id的对应关系，和关系的访问这两个功能即可

```solidity
pragma solidity ^0.4.20;

contract Adoption{
    // 一共有16只宠物的地址
    address[16] public adoptions;

    function adopt(uint petId) public returns (uint){
        require(petId >= 0 && petId <= 15);
        adoptions[petId] = msg.sender; // 谁发送的这条消息，谁就是宠物的领养者
        return petId;
    }

    function getAdoptions() public view returns (address[16]){
        return adoptions;
    }
}

```

- ### 编译

代码编写完成，就可以进行编译了，编译的过程也很简单,只需要在pet-shop (项目根目录下)执行 ```truffle compile ``` 即可

```shell
~/project/solidity/pet-shop ᐅ truffle compile
Compiling ./contracts/Adoption.sol...
Writing artifacts to ./build/contracts

~/project/solidity/pet-shop ᐅ cd ./build/contracts
~/project/solidity/pet-shop/build/contracts ᐅ ls
Adoption.json   Migrations.json
~/project/solidity/pet-shop/build/contracts ᐅ
```