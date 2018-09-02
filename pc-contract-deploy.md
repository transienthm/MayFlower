# 一、MetaMask安装

在chrome中安装MetaMask插件，MetaMask是一个轻量级以太坊钱包

具体安装教程：http://8btc.com/thread-76137-1-1.html

# 二、配置Network为私链

![image](https://user-images.githubusercontent.com/16509581/44954073-201cf780-aed0-11e8-8b69-cc89875af263.png)

![image](https://user-images.githubusercontent.com/16509581/44954120-62decf80-aed0-11e8-832c-9a18ce83da74.png)

rpc url的格式为http://ip:port 

# 三、remix中使用私链环境

![image](https://user-images.githubusercontent.com/16509581/44954188-b3eec380-aed0-11e8-836f-e6404ffa0bad.png)

需要注意的是，在私链部署时，需要在rpcapi中添加web3，如果需要私链挖矿，同样需要添加miner

```
geth --datadir data --networkid "201808" --rpc --rpcaddr "0.0.0.0" --rpcport 8546 --rpccorsdomain "*" --rpcapi="db,eth,net,web3,personal,miner"
```

此时需要输入rpc url，可能会有个大坑，如果remix是https连接，将无法成功连接rpc，因此需要将remix改为http连接

# 四、创建账户

1. 远程连接私链

   ```
   geth attach http://39.104.114.86:8546
   ```

2. 创建新账户

   ```
   personal.newAccount("password")
   ```

3. 设置解锁时间（在私链上可以设置得久一点）

   ```
   personal.unlockAccount(eth.accounts[0],"password",100000000)	
   ```

   另：eth.accounts[0]可以使用具体的地址来代替

4. 启动挖矿

   ```
   miner.start(2)
   ```

   其中，start的参数为线程数。用不了多久，就可以看到自己的账户余额发生了改变，然后可以任性地部署合约了！

   ![image](https://user-images.githubusercontent.com/16509581/44954292-5b202a80-aed2-11e8-8731-6d2a34a4fd8e.png)

