layout: python_list_layout
title: Python list的简单实现
date: 2017-05-24 17:12:05
tags: 重要
---
### Python list的简单实现

> 这里我主要实现了一下几个功能: append, pop, index, length写的比较乱
> 思路上大致就是

> * 设计每个节点的属性, 最简单的是每个几点只保存当前的值和指向下一个节点点指针, 我这里增加了当前节点的index, 以及指向前一个节点的指针
> * 设计链表的操作, append, pop, index, length
> * 可以在此基础上, 再实现insert, extend 等操作

```python
                
        #!/usr/bin/env python
        # coding: utf-8
        
        
        class Node(object):
        
            def __init__(self, value, next_node=None, pre_node=None):
                self.value = value
                self.index = 0
                self.next_node = next_node
                self.pre_node = pre_node
        
            def get_next_node(self):
                return self.next_node
        
            def set_next_node(self, new_node):
                self.next_node = new_node
        
            def get_pre_node(self):
                return self.pre_node
        
            def set_pre_node(self, pre_node):
                self.pre_node = pre_node
        
            def set_index(self, num):
                self.index = num
        
            def __str__(self):
                return str(self.value)
        
        
        class MyList(object):
        
            def __init__(self):
                self.start = Node
                self.head = None
                self.size = 0
        
            def _current_node(self):
                if not self.head:
                    current = None
                else:
                    current = self.head
                    while current.get_next_node():
                        current = current.get_next_node()
                return current
        
            def init_head(self):
                c = None
                index = 0
                if self.start:
                    c = self.start
                    c.index = 0
                    while c.get_next_node():
                        c = c.next_node
                        index += 1
                        c.index = index
                return c
        
            def append(self, item):
                node = Node(item)
                node.set_index(self.size)
                node.set_pre_node(self.head)
                current = self._current_node()
                if current:
                    current.set_next_node(node)
                else:
                    self.start = node
                self.head = node
                self.size += 1
                return 0
        
            def pop(self, index=None):
                if index > self.size - 1 or index < 0:
                    return None
                elif index is None or index == self.size - 1:
                    if not self.head:
                        return None
                    head = self.head
                    self.head = head.get_pre_node()
                    self.head.set_next_node(None)
                    return head
                else:
                    if self.start:
                        c = self.index(index)
                        c_next = c.get_next_node()
                        c_pre = c.get_pre_node()
                        if c_pre:
                            c_pre.set_next_node(c_next)
                        if c_next:
                            c_next.set_pre_node(c_pre)
                        self.head = self.init_head()
                        return c
                    else:
                        return None
        
            def index(self, num):
                c = self.start
                if c.index == num:
                    return c
                while c.get_next_node():
                    c = c.next_node
                    if c.index == num:
                        break
                return c
        
            @property
            def length(self):
                return self.size
        
            def __str__(self):
                string = '['
                if self.start:
                    string += str(self.start.value)
                    c = self.start
                    while c.get_next_node():
                        c = c.next_node
                        string += ', ' + str(c.value)
                string += ']'
                return string
        
        
        if __name__ == '__main__':
            l = MyList()
            l.append(9)
            l.append(10)
            l.append(90808098)
            l.append('asdjkad')
            l.append('ssss')
            print l, l.length
            print l.index(0)
            print l.pop(0)
```
