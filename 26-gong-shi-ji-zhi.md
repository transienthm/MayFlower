# 2.6 共识机制

共识机制的原理是，当一个节点通过工作量证明打包了区块添加到了区块链上，要和全网其他同时完成工作量证明的节点竞争最长链，如果发现比自己长的链，则替换掉自己的，基于这个，来完成我们的代码编写

```python
# BlockChain 
    def resolve_conflicts(self):
        neighbours = self.nodes

        max_len = len(self.chain)
        new_chain = None

        for node in neighbours:
            # 遍历节点信息
            response = requests.get(f'http://{node}/chain')
            if response.status_code == 200:
                length = response.json()['length']
                chain = response.json()['chain']

                if len(chain) > max_len and self._valid_chain(chain):
                    max_len = length
                    new_chain = chain

        if not new_chain:
            return False

        self.chain = new_chain
        return True
```