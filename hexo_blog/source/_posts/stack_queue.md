layout: structure_layout
title: 用两个栈实现一个队列
date: 2018-01-02 11:50:05
tags: 重点
comments: true
categories: Structure

---

### 栈
后进先出（Last In First Out），简称为LIFO线性表
栈调用图：![](http://dl.wcar.net.cn/jpg/2018/1/2//Users/kouyalong/Pictures/1340013975_1616.jpg)

#### 栈的实现

```python
# -*- coding: utf-8 -*-


class MyStackNode(object):

    def __init__(self, node, next_node=None):
        self.node = node
        self.next_node = next_node


class MyStack(object):

    def __init__(self):
        self.head = None
        self.size = 0

    def push(self, stack_node):
        stack_node.next_node = self.head
        self.head = stack_node
        self.size += 1

    def pop(self):
        ret = self.head
        if ret:
            self.head = self.head.next_node
            self.size -= 1
        return ret.node
```

#### 汉诺塔问题：
汉诺塔永远只有三步或者重复这三部：

![](http://dl.wcar.net.cn/jpg/2018/1/2//Users/kouyalong/Pictures/v2-a3c945db9b5a2e8fd455bbc4c7fbce35_hd.jpg)

![](http://dl.wcar.net.cn/jpg/2018/1/2//Users/kouyalong/Pictures/v2-4d14e3c3e893135a9d26fdf11984b493_hd.jpg)

![](http://dl.wcar.net.cn/jpg/2018/1/2//Users/kouyalong/Pictures/v2-7a2c13480cf2c9ab79aa490df4a642ac_hd.jpg)

![](http://dl.wcar.net.cn/jpg/2018/1/2//Users/kouyalong/Pictures/v2-24990a5accb631335dc665f6356b8e55_hd.jpg)

![](http://dl.wcar.net.cn/jpg/2018/1/2//Users/kouyalong/Pictures/v2-fc54c46260c35a0e894a26f74b1735af_hd.jpg)

代码实现

```python
# -*- coding: utf-8 -*-

def move(n, a, buffer_, c):
    if n == 1:
        print a, "->", c
        return
    # 打开冰箱门
    move(n-1, a, c, buffer_)
    # 把大象装进去
    move(1, a, buffer_, c)
    # 关上冰箱门
    move(n-1, buffer_, a, c)

move(5, "A", "B", "C")
```


### 队列
队列(Queue)也是一种运算受限的线性表，它的运算限制与栈不同，是两头都有限制，插入只能在表的一端进行(只进不出)，而删除只能在表的另一端进行(只出不进)，允许删除的一端称为队尾(rear)，允许插入的一端称为队头 (Front)

#### 队列的实现

```python
# -*- coding: utf-8 -*-


class QueueNode(object):

    def __init__(self, value):
        self.node = value
        self.next_node = None


class QueueInfo(object):

    def __init__(self):
        self.front = None
        self.rear = None
        self.size = 0

    def pop(self):
        ret = None
        if self.front:
            ret = self.front.node
            if self.front == self.rear:
                self.rear = None
            self.front = self.front.next_node
            self.size -= 1
        return ret

    def push(self, item):
        if item is None:
            raise Exception('Node is not None')
        new_node = QueueNode(item)
        if self.size == 0:
            self.rear = new_node
            self.front = new_node
        else:
            self.rear.next_node = new_node
            self.rear = new_node
        self.size += 1

```

### 用两个栈实现一个队列代码实现

```python
# -*- coding: utf-8 -*-


class MyStackNode(object):

    def __init__(self, node, next_node=None):
        self.node = node
        self.next_node = next_node


class MyStack(object):

    def __init__(self):
        self.head = None
        self.size = 0

    def push(self, stack_node):
        stack_node.next_node = self.head
        self.head = stack_node
        self.size += 1

    def pop(self):
        ret = self.head
        if ret:
            self.head = self.head.next_node
            self.size -= 1
        return ret.node


class MyQueue(object):

    def __init__(self):
        self.stack_one = MyStack()
        self.stack_two = MyStack()

    def pop(self):
        if self.stack_two.size > 0:
            return self.stack_two.pop()
        else:
            while self.stack_one.size > 0:
                stack_node = MyStackNode(self.stack_one.pop())
                self.stack_two.push(stack_node)
            if self.stack_two.size > 0:
                return self.stack_two.pop()
            else:
                return None

    def push(self, item):
        stack_node = MyStackNode(item)
        self.stack_one.push(stack_node)


def create_stack(seq):
    my_stack = MyStack()
    for item in seq:
        stack_node = MyStackNode(item)
        my_stack.push(stack_node)
    for i in range(my_stack.size):
        print my_stack.pop()

create_stack([1, 4, 5, 2, 8, 3])
```