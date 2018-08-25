# 2.4 添加节点通信功能

* #### 使用Flask启动web服务器

项目结构如下
```
├── Pipfile
├── Pipfile.lock
├── __pycache__
├── app
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-36.pyc
│   │   └── settings.cpython-36.pyc
│   ├── blockchain
│   │   ├── __init__.py
│   │   ├── __pycache__
│   │   │   ├── __init__.cpython-36.pyc
│   │   │   └── blockchain.cpython-36.pyc
│   │   └── blockchain.py
│   ├── settings.py
│   └── web
│       ├── __init__.py
│       ├── __pycache__
│       │   ├── __init__.cpython-36.pyc
│       │   └── communication.cpython-36.pyc
│       └── communication.py
└── blockchain.py


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