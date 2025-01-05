---
tags: 
progress: 
creation date: 2024-09-01 19:29
modification date: 星期日 1日 九月 2024 19:29:14
---
[用FastAPI和Vue.js开发一个单页应用程序下面将逐步介绍如何使用FastAPI、Vue、Docker和Postg - 掘金](https://juejin.cn/post/7113790977848360967)



我现在有三张表，user address order , address 的外键关联到user的主键，order 的外键关联到address的主键。

我该如何删掉一张 表


Base = declarative_base()

  

class User(Base):

__tablename__ = "users"

  

id = Column(Integer, primary_key=True, index=True)

name = Column(String(100), nullable=False)

email = Column(String(100), unique=True, index=True, nullable=False)

  

# 定义与 Address 的关系

addresses = relationship("Address", back_populates="owner", cascade="all, delete-orphan")

  
  

class Address(Base):

__tablename__ = "addresses"

  

id = Column(Integer, primary_key=True, index=True)

street = Column(String(100), nullable=False)

city = Column(String(50), nullable=False)

owner_id = Column(Integer, ForeignKey("users.id"))

  

# 定义与 User 的关系

owner = relationship("User", back_populates="addresses")

myorder = relationship("Order", back_populates = "myaddress",cascade = "all,delete-orphan")

  

class Order(Base):

__tablename__ = "orders"

  

id = Column(Integer, primary_key=True, index=True)

order_info = Column(String(50), nullable=False)

address_id = Column(Integer, ForeignKey("addresses.id"))

myaddress = relationship("Address",back_populates = "myorder")
新建用户时 为什么会报这样的错呢

 File "/home/orange/.local/lib/python3.10/site-packages/sqlalchemy/orm/mapper.py", line 2405, in _post_configure_properties
    prop.init()
  File "/home/orange/.local/lib/python3.10/site-packages/sqlalchemy/orm/interfaces.py", line 584, in init
    self.do_init()
  File "/home/orange/.local/lib/python3.10/site-packages/sqlalchemy/orm/relationships.py", line 1647, in do_init
    self._generate_backref()
  File "/home/orange/.local/lib/python3.10/site-packages/sqlalchemy/orm/relationships.py", line 2133, in _generate_backref
    self._add_reverse_property(self.back_populates)
  File "/home/orange/.local/lib/python3.10/site-packages/sqlalchemy/orm/relationships.py", line 1578, in _add_reverse_property
    other = self.mapper.get_property(key, _configure_mappers=False)
  File "/home/orange/.local/lib/python3.10/site-packages/sqlalchemy/orm/mapper.py", line 2511, in get_property
    raise sa_exc.InvalidRequestError(
sqlalchemy.exc.InvalidRequestError: Mapper 'Mapper[Address(addresses)]' has no property 'addresses'.  If this property was indicated 