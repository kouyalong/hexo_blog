layout: normal_layout
title: Python常见问题
date: 2017-06-13 18:12:05
tags: 随便
comments: true
categories: Python

---

#### 列表推导(List comprehensions)
列表推导提供了一种创建列表的简洁方法

* 生成一个list
    ```python
    seq = [x**2 for x in range(10)]
    ```

* 获取一个目录下文件名
找到py结尾的文件
    ```python
    import os
    file_names = [fn for fn in os.listdir('./') if fn.endswith('.py')]
    ```

#### 匿名函数

求一个数的平方
```python
func = lambda x: x**2
```

斐波那契数列
```python
fib = lambda n: 1 if n <= 2 else fib(n-1)+fib(n-2)
```

#### 偏函数

#### 迭代器

#### 生成器

#### 装饰器

#### 多线程

#### 协程

#### GC

#### 猴子补丁

