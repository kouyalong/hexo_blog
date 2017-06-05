layout: four
title: Mysql使用心得
date: 2016-11-22 23:16:05
tags: 重点
---
# MYSQL
MySql是一个快速、多线程、多用户的SQL数据库服务器。几乎是免费的，支持正规的SQL查询语言和采用多种数据类型，能对数据进行各种详细的查询等。

![](https://martin-upload.b0.upaiyun.com/web/2016/11/3cdb883804bf06eb6c5326317d2fbc60.png)

## Relational Database Management System

#### DDL（Data Definition Language）
CREATE、ALTER与DROP

#### DML（Data Manipulation Language）
插，删，改，排，检
INSERT、DELETE、JOIN、SORT、SELECT

> #### 注意
> 1.between and 的边界
2.通配符：%和_
3.limit + offset
4.inner join 和 left join
5.union 和 union all


# REDIS


# MONGO


# SQLALCHEMY
![](https://martin-upload.b0.upaiyun.com/web/2016/11/2bc29ffdc76fd28affe82473665cce4c.png)
* <font color=red>SQLAlchemy无法修改表结构，如果需要可以使用SQLAlchemy开发者开源的另外一个软件Alembic来完成</font>

#### 1.create_engine
A.
strategy.create的调用，而 strategy 就是 engine 的实现细节. strategy 可以在 create_engine 调用时通过 strategy 参数指定, 目前官方的支持有三种：
* plain, 默认的
* threadlocal, 连接是线程局部的
* mock, 所有的 SQL 语句的执行会使用指定的函数

B.
```
- pool_size 连接数
- max_overflow 最多多几个连接
- pool_recycle 连接重置周期
- pool_timeout 连接超时时间
```
```
from sqlalchemy import create_engine


def f(sql,*args,**kargs):
    print sql, args, kargs

s= 'mysql://user:password@host:port/database?charset=utf-8'
engine = create_engine(s, strategy='mock', executor=f, echo=False, pool_size=10, max_overflow=5, pool_recycle=3600, pool_timeout=30)
print engine.execute('select id from "user"')
```

#### 2. table
SQLAlchemy的Delarative
A.
SQLAlchemy使用 Classic和Modern两种模式 来描述一个表。

Classic
```
from sqlalchemy import Column, Integer, MetaData, String, Table
from sqlalchemy.orm import mapper

metadata = MetaData()

user = Table('users', metadata,
            Column('id', Integer, primary_key=True),
            Column('name', String(50)),
            Column('fullname', String(50)),
            Column('password', String(12))
        )


class User(object):
    def __init__(self, name, fullname, password):
        self.name = name
        self.fullname = fullname
        self.password = password

mapper(User, user)
```
Modern
```
from sqlalchemy import Column, Integer, MetaData, String
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True)
    name =  Column(String(50))
    fullname = Column(String(50))
    password = Column(String(12))

    def __init__(self, name, fullname, password):
        self.name = name
        self.fullname = fullname
        self.password = password

    def __repr__(self):
        return "<User('%s','%s', '%s')>" % (self.name, self.fullname, self.password)
user = User()
```
ORM会为所有被映射的表列创建类属性，这些属性是通过Python语言中[python描述符](http://www.geekfan.net/7862/)机制来实现的。
SQLAlchemy 的连接创建是 Lazy 的方式。要对对象实行持久化(Persist) (所谓的持久化就是把对象数据按照映射关系存储入数据库里)

#### 3.session
ORM的操作句柄(Handle)被称为会话(Session)。

```
from contextlib import closing

from sqlalchemy.orm import sessionmaker

Session = sessionmaker()
Session.configure(bind=engine, autocommit=False, expire_on_commit=True)
with closing(Session()) as session:
    user = User()
    user.name = 'test'
    session.add(user)
    session.commit()
```