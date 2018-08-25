# 2.5 实现注册节点

实现注册节点这个功能比较简单，就是在我们的BlockChain类中 维护一个成员变量nodes，其类型是Set。然后通过Http接口接受穿过来的地址，将其保存到nodes里

```python
class Blockchain:

    def __init__(self):
        self.chain = []
        self.current_transactions = []
        self.nodes = set()

        # 创始区块 工作量证明随意，上一个区块hash值随意
        self.new_block(proof=100, previous_hash=1)
        
    def register_node(self, address):
        parsed_url = urlparse(address)
        self.nodes.add(parsed_url.netloc)
        
    ...
    ...
```

路由函数
```python
@web.route('/node/register', methods=['POST'])
def register_node():
    values = request.get_json()
    nodes = values.get("nodes")

    if nodes is None:
        return "Error: please supply a valid list of nodes", 400

    for node in nodes:
        block_chain.register_node(node)

    response = {
        'message': 'New Nodes have been added',
        'total_nodes': list(block_chain.nodes)
    }

    return jsonify(response), 201
```