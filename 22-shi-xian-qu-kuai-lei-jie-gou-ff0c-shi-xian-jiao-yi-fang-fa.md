# 2.2 实现区块类结构，实现交易方法

```python
"""
create by gaowenfeng on 2018/8/25
"""
import hashlib
import json

__author__ = "gaowenfeng"

from time import time

"""
区块的结构
{
    "index": 0, 索引
    "timestamp": "", 时间戳
    "transactions": [
        {
            "sender": "",
            "recipient": "",
            "amount": 5
        }
    ],
    "proof": "", # 工作量证明
    "previous_hash": "" # 上一个区块的hash
}
"""


class Blockchain:

    def __init__(self):
        self.chain = []
        self.current_transactions = []

        # 创始区块 工作量证明随意，上一个区块hash值随意
        self.new_block(proof=100, previous_hash=1)

    def new_block(self, proof, previous_hash=None):

        block = {
            'index': len(self.chain) + 1,  # 链的长度+1
            'timestamp': time(),
            'transactions': self.current_transactions,
            'proof': proof,
            'previous_hash': previous_hash or self.hash(self.last_block)
        }

        self.current_transactions = []
        self.chain.append(block)

        return block

    def new_transaction(self, sender, recipient, amount):
        self.current_transactions.append({
            'sender': sender,
            'recipient': recipient,
            'amount': amount
        })

        # 返回上一个区块的索引
        return self.last_block['index']+1

    @staticmethod
    def hash(block):

        block_string = json.dumps(block, sort_keys=True)

        # hexdigest 是hash过后的摘要信息
        return hashlib.sha256(block_string).hexdigest

    @property
    def last_block(self):
        return self.chain(-1)  # -1 数组里的最后一个元素




```