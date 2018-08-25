# 2.2 实现区块类结构，实现交易方法

```
"""
create by gaowenfeng on 2018/8/25
"""

__author__ = "gaowenfeng"

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

    def new_block(self):
        pass

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
        pass

    @property
    def last_block(self):
        pass



```