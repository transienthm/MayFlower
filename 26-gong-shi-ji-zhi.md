# 2.6 共识机制

共识机制的原理是，当一个节点通过工作量证明打包了区块添加到了区块链上，要和全网其他同时完成工作量证明的节点竞争最长链，如果发现比自己长的链，则替换掉自己的，基于这个，来完成我们的代码编写

- BlockChain

```python  
    def resolve_conflicts(self):
        neighbours = self.nodes

        max_len = len(self.chain)
        new_chain = None

        for node in neighbours:
            # 遍历邻居节点，获取他们的链信息
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
        
    def _valid_chain(self, chain):
        last_block = chain[0]
        current_index = 1
        
        以此遍历链上的每一个节点
        while current_index < len(chain):
            block = chain[current_index]
            # 如果某个节点保存的上一个节点的hash值不正确，则校验不通过
            if block['previous_hash'] != self.hash(last_block):
                return False

            # 如果某个节点不能通过工作量证明，则校验不通过
            if not self._valid_proof(last_block['proof'], block['proof']):
                return False

            last_block = block
            current_index += 1

        return True
```

- 视图函数
```python
@web.route('/node/resolve', methods=['GET'])
def consensus():
    replaced = block_chain.resolve_conflicts()

    if replaced:
        response = {
            'message': 'Our chain was replaced',
            'new_chain': block_chain.chain
        }
    else:
        response = {
            'message': 'Our chain is authoritative',
            'chain': block_chain.chain
        }

    return jsonify(response)
```

至此，我们的简易区块链就完成了