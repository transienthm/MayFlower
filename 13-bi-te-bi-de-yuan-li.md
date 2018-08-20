# 1.3 比特币的原理-账本如何验证


在中心化的系统里面，如果数据被篡改了，几乎是没有办法验证的，因为他的数据存储在一方，其他人没有办法获取和验证。在分布式的去中心化系统里面，如比特币。每个节点（可以是一个用户的一台电脑（旷工），也可以是一组服务器集群（矿池））都有一份账本的全部信息。这样如果其中一个节点修改了数据，其他节点就会验证不通过（具体的保证机制后面会介绍），也就保证了信息的不可篡改性

以比特币为例，比特币每10分钟生成一个新的账本，这个账本里保存了10分钟内的所有交易记录。每个节点都会得到这样一份交易记录，并对这个记录进行hash，得到一个hash值，然后节点之间用这个hash值进行互相验证。
> hash函数
同样的原始信息用同一个哈希函数总能得到相同的摘要信息
原始信息任何微小的变化都会哈希出面目全非的摘要信息
从摘要信息无法逆向推算出原始信息

![image.png](https://upload-images.jianshu.io/upload_images/7220971-d58ec2c4b0d56192.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

每一个账本的序号，hash值，时间戳，和交易的原始的交易记录组合在一起，就形成了一个区块，而且我们叫序号，hash值，时间戳称为这个区块的头

![image.png](https://upload-images.jianshu.io/upload_images/7220971-b4fb98b8ddfca57d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当下一个10分钟有产生了一个交易记录账本。那么就会将上一个区块的hash值和这次的账本共同进行hash产生一个hash值，然后进而产生一个新的区块

![image.png](https://upload-images.jianshu.io/upload_images/7220971-7f28c86f2f9c256c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以此类推，按照这样的方式就会产生第三个快，第四个快。。。第N个块,所有这些块串联的结构就成为区块链。

然后每一个节点在核对数据的时候，只需要核对最后一个块的摘要信息，如果能够核对上的话，就说明整个区块链的账本是正确的

![image.png](https://upload-images.jianshu.io/upload_images/7220971-8f1a2269b6dbfb31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)









