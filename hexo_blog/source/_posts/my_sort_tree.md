layout: my_sort_tree_layout
title: Python 二叉查找树 二叉树遍历
date: 2017-07-03 15:12:05
tags: 重要
comments: true
categories: Python

---

#### 二叉查找树

> 二叉排序树或者是一棵空树，或者是具有下列性质的二叉树：
>   （1）若左子树不空，则左子树上所有结点的值均小于或等于它的根结点的值；
    （2）若右子树不空，则右子树上所有结点的值均大于或等于它的根结点的值；
    （3）左、右子树也分别为二叉排序树；

下面是主要的数显代码

```python
#!/usr/bin/env python
# coding: utf-8

import random


class TreeNode(object):
    """
    二叉树的基本数据结构
    """
    def __init__(self, value=None, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

    def set_left(self, left):
        self.left = left

    def set_right(self, right):
        self.right = right


class MyTree(object):
    """
    根据一个数据列表, 创建一个二叉查找树
    """
    def __init__(self, tree_list):
        self.root = self.create_tree(tree_list)
        self.pre_list = list()
        self.in_list = list()
        self.after_list = list()

    def insert_child(self, root, child):
        if root:
            if root.value >= child.value:
                if root.left:
                    self.insert_child(root.left, child)
                else:
                    root.set_left(child)
            else:
                if root.right:
                    self.insert_child(root.right, child)
                else:
                    root.set_right(child)
        else:
            return None

    def create_tree(self, tree_list):
        if tree_list:
            root = TreeNode(value=tree_list[0])
        else:
            return None
        for item in tree_list[1:]:
            print item
            child = TreeNode(value=item)
            self.insert_child(root, child)
        return root

    def pre_order(self, root):
        """
        先序遍历
        """
        if root:
            self.pre_list.append(root.value)
            self.pre_order(root.left)
            self.pre_order(root.right)

    def in_order(self, root):
        """
        中序遍历
        """
        if root:
            self.in_order(root.left)
            self.in_list.append(root.value)
            self.in_order(root.right)

    def after_order(self, root):
        """
        后序遍历
        """
        if root:
            self.after_order(root.left)
            self.after_order(root.right)
            self.after_list.append(root.value)

numbers = [random.choice(range(100)) for i in xrange(10)]
print numbers
my_tree = MyTree(numbers)
my_tree.pre_order(my_tree.root)
print my_tree.pre_list
my_tree.in_order(my_tree.root)
print my_tree.in_list
my_tree.after_order(my_tree.root)
print my_tree.after_list

```