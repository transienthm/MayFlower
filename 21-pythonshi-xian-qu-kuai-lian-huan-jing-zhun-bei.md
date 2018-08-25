# 2.1 Python实现区块链环境准备

- #### HTTP协议的理解
- #### PostMan的安装 https://www.getpostman.com/
- #### Python https://www.python.org/
- #### Pycharm(Python IDE) https://www.jetbrains.com/pycharm/
- #### Pip（安装管理python包的一个工具）
- #### pipenv （提供Python的虚拟运行环境）
- #### Flask & Request（python web开发）

```
pip install pipenv
cd ~/your_workspace
mkdir blockchain_principle
cd blockchain_principle
pipenv shell
pipenv install flask==1.0
pipenv install requests==2.18.4
```

创建blockchain.py文件，编写区块结构

```python
"""
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

```
