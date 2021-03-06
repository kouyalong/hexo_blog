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

lambda 用来编写简单的函数

求一个数的平方
```python
func = lambda x: x**2
```

斐波那契数列
```python
fib = lambda n: 1 if n <= 2 else fib(n-1)+fib(n-2)
```


#### 偏函数
python的functools提供了partial, 可以让我们通过设定参数的默认值，可以降低函数调用的难度
下面就是一个求一个数的number次方的例子

```python
#!/usr/bin/env python
# coding: utf-8

from functools import partial


def square(n, number):
    return n ** number


new_square = partial(square, number=2)


print square(9, 2)

print new_square(9)
```

#### 迭代器

##### 可迭代对象


#### 生成器

#### 装饰器

#### 多线程

#### 协程(Coroutine)

协程看上去也是子程序. 但执行过程中, 在子程序内部可中断, 然后转而执行别的子程序, 在适当的时候再返回来接着执行.
最大的优势就是协程极高的执行效率. 因为子程序切换不是线程切换, 而是由程序自身控; 因此, 没有线程切换的开销, 和多线程比, 线程数量越多, 协程的性能优势就越明显.
第二大优势就是不需要多线程的锁机制, 因为只有一个线程, 也不存在同时写变量冲突, 在协程中控制共享资源不加锁, 只需要判断状态就好了, 所以执行效率比多线程高很多.

#### GC

为了解决内存泄露问题, 采用了对象引用计数, 并基于引用计数实现自动垃圾回收.
Python的gc有比较强的功能, 比如设置 gc.set_debug(gc.DEBUG_LEAK)就可以进行循环引用导致的内存泄露的检查.
如果在开发时进行内存泄露检查, 在发布时能够确保不会内存泄露, 那么就可以延长Python的垃圾回收时间间隔, 甚至主动关闭垃圾回收机制, 从而提高运行效率.

#### 猴子补丁

不改变源代码而对功能进行追加和变更. 充分利用了动态语言的灵活性, 可以对现有的语言Api进行追加, 替换, 修改Bug, 甚至性能优化等等.
如果要使用猴子补丁, 那么只是做功能追加, 尽量避免大规模的API覆盖.

#### 多继承

多重继承时, 用于在子类中调用父类方法时确定调用哪个父类的方法.

[C3算法](http://www.cnblogs.com/i2u9/archive/2013/03/19/pythonmroc3.html)和`__mro__`

````python

````

#### MixIn

在不改变原对象的基础下对其进行扩展. 在运行期间, 动态改变类的基类或者类的方法, 从而使得类的表现可以发生变化.


