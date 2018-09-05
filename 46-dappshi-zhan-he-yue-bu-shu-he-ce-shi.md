### 4.6 DAPP 合约部署和测试

- ### 合约部署

1.修改truffle.js

```js
/*
 * NB: since truffle-hdwallet-provider 0.0.5 you must wrap HDWallet providers in a 
 * function when declaring them. Failure to do so will cause commands to hang. ex:
 * 
 * mainnet: {
 *     provider: function() { 
 *       return new HDWalletProvider(mnemonic, 'https://mainnet.infura.io/<infura-key>') 
 *     },
 *     network_id: '1',
 *     gas: 4500000,
 *     gasPrice: 10000000000,
 *   },
 */

module.exports = {
  // See <http://truffleframework.com/docs/advanced/configuration>
  // to customize your Truffle configuration!
  networks: {
    development: { //要部署到的结点信息
      host: "39.104.114.86",  
      port: 8546,
      network_id: "*" // Match any network id
    }
  }
};
```

2.在migrations目录编写部署脚本

```js
var Adoption = artifacts.require("Adoption"); // require的参数是合约名

module.exports = function(deployer) {
  // deployment steps
  deployer.deploy(Adoption); // 部署合约
};

```


3.执行truffle migrate 部署
```shell
~/project/solidity/pet-shop ᐅ truffle migrate
Using network 'development'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0x94b385261ca5cefda0202e43726dcfa46aabbbf85de6e331096a478fae710a85
  Migrations: 0x1a3b3022ccf9ae81624d958b2eef408ccb357dd3
Saving successful migration to network...
  ... 0x930cc19a0fb6df4e64c81453f9bbdd72df9849acda4bc013d852f97ac9b9a176
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying Adoption...
  ... 0x60d97737d8db67c008e6a59fd7570b23036bd3e6a5489a433b550c0df0dade50
  Adoption: 0xa776e0cd0a3d6c709de0dd84d1eea9a9ac3a4707
Saving successful migration to network...
  ... 0x3221faf90b22be14c656c81a774be1b0c145710d7923113dc1dd544b37590fd7
Saving artifacts...
```
部署成功后观察 build/contracts/ 目录下的合约json文件的里的networks下多了相关信息


- ### 测试

1.在test文件夹下编写测试的合约TestAdoption.sol

```solidity
pragma solidity ^0.4.17;

import 'truffle/Assert.sol'; // 引入断言
import 'truffle/DeployedAddresses.sol'; // 通过这个可以获取到部署合约的地址
import '../contracts/Adoption.sol';

contract TestAdoption {
    
    Adoption adoption = Adoption(DeployedAddresses.Adoption());

    function testUserCanAdoptPet() public {
        uint returnedId = adoption.adopt(8);
        uint expected = 8;
        Assert.equal(returnedId, expected, "Aoption of pet Id 8 should be recorded.");
    }

    function testGetAdopterAddressByPetid() public {
        address expected = this;
        address adopter = adoption.adopters(8);

        Assert.equal(adopter, expected, "Owner of pet ID 8 should be recorded.");
    }

    function testGetAdopterAddressByPetIdInArray() public {

        address expected = this;

        address[16] memory adopters = adoption.getAdopters();
        Assert.equal(adopters[8], expected, "Owner of Pet Id 8 should be recoded.");
    }
}


```

2.执行测试,执行的过程中会花费gas

```shell
~/project/solidity/pet-shop ᐅ truffle test
Using network 'development'.

Compiling ./contracts/Adoption.sol...
Compiling ./test/TestAdoption.sol...
Compiling truffle/Assert.sol...
Compiling truffle/DeployedAddresses.sol...


  TestAdoption
    ✓ testUserCanAdoptPet (82ms)
    ✓ testGetAdopterAddressByPetid (139ms)
    ✓ testGetAdopterAddressByPetIdInArray (120ms)


  3 passing (1s)
```

