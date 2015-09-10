# mdum - Multi Database Unified Modeling
--------

[中文文档](https://github.com/Feiox/mdum/blob/master/README_zh.md)

| Name:       | Multi Database Unified Modeling    |
|-------------|------------------------------------|
| Author:     | Feiox                              |
| Maintainer: | Feiox \<[Fei2037@gmail.com](mailto:fei2037@gmail.com)\> |

## About

mdum is a Python Object-Document Mapper for working with Redis Elasticsearch MongoDB.
Documentation available at [Read the Docs](http://mdum.rtfd.org) - there is currently
a [tutorial](https://readthedocs.org/docs/mdum/zh_CN/latest/tutorial.html), a [feature](https://readthedocs.org/docs/mdum/zh_CN/latest/feature.html), a [develop](https://readthedocs.org/docs/mdum/zh_CN/latest/develop.html) and an [API reference](https://readthedocs.org/docs/mdum/zh_CN/latest/apireference.html).


### Philosophy

* Data Modeling: Declarative DSL is better than procedural 
* Interface: Simple, Clear, Semantic, Stratified
* Expansibility: Don't need to understand the internal implementation
* Life: `JUST FOR FUN`

### Feature

* Semantic & like Python syntax DSL
* Operating data, like Python built-in objects
* Use multiple databases in a uniform interface
* High performance, Cache precompiled all DSL, Model, and so on

## Base Usage

### Installation

We recommend the use of virtualenv and pip. You can then use `pip install mdox`. Otherwise, you can download the source from GitHub and run `python setup.py install`.
Be sure to check the version of dependent packages when you install, low version will lead to unpredictable errors.

### Dependencies

python interpreter:

* python2 >= 2.7
* python3 >= 3.4

python packages:

* pymongo >= 3.0
* elasticsearch >= 1.6
* redis >=2.10

Database Softwares:

* MongoDB >= 3.0
* Elasticsearch >= 1.7
* Redis >= 3.0

#### Optional Dependencies

TODO

### Examples
To run the examples, ensure you are running a local instance of MongoDB and Elasticsearch on the standard port.
Create and initialize mdum app code looks like:

```python
import mdum

db_app = mdum.MdumApp().init()  # use default configuration
```

Some simple examples of what model definition code looks like:

```python
from mdum import Document
from mdum.fields import String, Int, Datetime, Identifier, GeoPosition

class UserModel(Document):
    __meta__ = [
        'indexes': ['-username']
        'triggers': [
            # assuming that these function has been defined
            'after_create': init_new_user  
            'pre_delete': clear_up_user_data
        ]
    ]
    
    # will be used at Elasticsearch
    doc_schema = {
        'username': String(required=True),
        'desc': String(length_scope=[1, 255]),
    }
    # will be used at MongoDB
    data_schema = {
    	'role': Enum(['rookie', 'user', 'vip'], default='rookie'),
    	'friends': List(type=Identifier),
    	'location': GeoPosition(),
    	'join_time': Datetime(default_now=True),
    	'department': Identifier(join=True, required=True),
    }

```
If you want to operate data, you can try:

```
>>> UserModel.create({'username': 'Lynn', 'department': 32717})
4e7020cb7cac81af7136236b
>>> book = UserModel(pk='4e7020cb7cac81af7136236b')  # or UserModel.get('4e7020cb7cac81af7136236b')
{'username': 'Lynn', 'department': {'id': 32717, 'name': 'Engineering'}, 'time': datetime(2015, 9, 3, 12, 31, 56), 'role': 'rookie'}
>>> book.update({'department': 21345}, dont_return=True)
True
>>> book.delete('4e7020cb7cac81af7136236b')
True
>>> Book.find('join_time >= {} or friends size is {comments_num}', datetime(2015, 9, 3, 12, 31, 56), comments_num=10)
ResultSet()
```
Well, the coding should be simple and fun.

## Fork & Contribution

### Contribute Code or Idea
We welcome contributions! see the [Contribution guidelines](https://readthedocs.org/docs/mdum/zh_CN/latest/contribution.html).

### Fork Me

### Donate Me

Now don't need donations yet :P

### License

Mdum is licensed under very liberal Apache license, then any private or commercial purposes can be permitted.

Copyright (c) 2015, Feiox All rights reserved.

## Thanks

* My Lover, Qiucen
* MongoEngine
