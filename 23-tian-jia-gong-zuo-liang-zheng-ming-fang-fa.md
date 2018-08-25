# 2.3 添加工作量证明方法

回顾一下工作量证明的原理，对交易信息进行hash，并引入随机字符串来得到hash结果，对hash结果进行难度限制，限制必须以n个0开头。这样节点需要消耗算力不断的尝试新的字符串得到符合条件的结果

```python
    def proof_of_work(self, last_proof):
        proof = 0
        # 工作量证明需要使用上一个区块的hash值，这里做一个简化版，拿上一个区块的工作量证明
        while self.valid_proof(last_proof, proof) is False:
            proof += 1
        print("proof:%s" % proof)
        return proof

    def valid_proof(self, last_proof, proof):
        guess = f'{last_proof}{proof}'.encode()
        guess_hash = hashlib.sha256(guess).hexdigest()

        print("guess_hash:%s" % guess_hash)
        return guess_hash[0:4] == PROOF_DIFFICULTY
        
    # 其中的print语句是为了观察测试使用，可以删掉
```