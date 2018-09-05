# 4.7 DAPP实战，合约与前端交互

合约与前端交互需要用到[web3.js](https://github.com/ethereum/web3.js)和[truffle-contract](https://github.com/trufflesuite/truffle-contract),web3.js 是用来和以太坊区块链做交互的，而truffle-contract封装了web3.js，提供了一些高级的js功能，其实web3.js也可以完成合约的编译部署调用全过程。但是truffle-contract更方便。


- web3.js

```JavaScript
    if (typeof web3 !== 'undefined') {
      App.web3Provider = web3.currentProvider;
    } else {
      App.web3Provider = new Web3.prviders.HttpProvider("http://127.0.0.1:7545");
    }
    web3 = new Web3(App.web3Provider);
```


- truffle-contract

首先访问合约的二进制json接口文件（编译的时候产生的），然后将其作为构造参数传入TruffleContract，生成TruffleContract对象，然后给TruffleContract设置web3provider。

经过这两步操作，TruffleContract就可以调用合约了。
```JavaScript
    $.getJSON('Adoption.json', function(data) {
      var AdoptionArtifact = data;

      App.contracts.Adoption = TruffleContract(AdoptionArtifact);
      App.contracts.Adoption.setProvider(App.web3Provider);

      return App.markAdopted();
    });
```

TruffleContract支持函数式编程，每一个then方法的return会作为下一个then的输入，具体使用如下

```JavaScript
    // 首先调用deployed方法，获取合约实例
    App.contracts.Adoption.deployed().then(function(instance) {
      apotionInstance = instance;
       // 然后就可以通过合约实例方案合约方法和属性了（getAdopters就是合约方法，具体参照4.4节）
      return apotionInstance.getAdopters.call();
    }).then(function(adopters) {
      
      for(i =0; i< adopters.length; i++) {
        console.log(adopters[i]);
        if(adopters[i] !== '0x0000000000000000000000000000000000000000') {
          $('.panel-pet').eq(i).find('button').text('Success').attr('disabled', true);
        }
      }
    }).catch(function(err) {
      console.log(err.message);
    })
```