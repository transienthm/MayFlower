# 2.4 添加节点通信功能
```
注：由于这里主要以实现区块链基本原理为目的，所以并没有对Flask进行过多设计和封装，敬请谅解
```
* #### 使用Flask启动web服务器

项目结构如下
```
├── Pipfile
├── Pipfile.lock
├── __pycache__
├── app
│   ├── __init__.py
│   ├── blockchain
│   │   ├── __init__.py
│   │   └── blockchain.py # 区块链核心结构实现
│   ├── settings.py
│   └── web
│       ├── __init__.py
│       └── communication.py # 路由文件
└── blockchain.py # 节点启动


```

```python
from flask import Flask

__author__ = "gaowenfeng"


def create_app():
    app = Flask(__name__)
    register_blueprint(app)

    return app


def register_blueprint(app):
    from app.web import web
    app.register_blueprint(web)
```

```python
from app import create_app

__author__ = "gaowenfeng"

app = create_app()

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

* #### 实现添加交易接口
```python
@web.route('/transactions/new', methods=['POST'])
def new_transaction():
    values = request.get_json()
    required = ['sender', 'recipient', 'amount']
    if values is None or not all(k in values for k in required):
        return "Missing values", 400

    index = block_chain.new_transaction(values['sender'],
                                        values['recipient'],
                                        values['amount'])

    response = {'message': f'Transaction will be add to Block {index}'}
    return jsonify(response), 201
```

* #### 实现挖矿接口
```python
@web.route('/mine', methods=['GET'])
def mine():
    last_block = block_chain.last_block
    last_proof = last_block['proof']
    proof = block_chain.proof_of_work(last_proof)

    block_chain.new_transaction(sender='0', recipient=node_identifier, amount=1)
    block = block_chain.new_block(proof, None)

    response = {
        'message': 'New Block Forged',
        'index': block['index'],
        'transactions': block['transactions'],
        'proof': block['proof'],
        'previous_hash': block['previous_hash']
    }

    return jsonify(response), 201
```