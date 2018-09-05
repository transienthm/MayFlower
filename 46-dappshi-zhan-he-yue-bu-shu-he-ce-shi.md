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
```
truffle migrat
```
部署成功后观察 build/contracts/ 目录下的合约json文件的里的networks下多了相关信息


- ### 测试

在test文件夹下编写测试的合约TestAdoption.sol

```
pragma solidity ^0.4.17;

import 'truffle/Assert.sol';
import 'truffle/DeployedAddresses.sol';
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