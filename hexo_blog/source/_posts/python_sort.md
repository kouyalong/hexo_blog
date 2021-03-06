layout: sort_layout
title: Python 实现八大排序
date: 2017-05-24 18:16:05
tags: 重点
comments: true
categories: Python

---
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
### 排序算法

* 各种排序算法
![](https://martin-upload.b0.upaiyun.com/web/2017/05/4097d5d86145141fcb6a5ecff0fbc3bc.jpg)

* 各种排序算法比较
![](https://martin-upload.b0.upaiyun.com/web/2017/05/746fe11d81c35fc57bb2b8ba17be65c5.jpg)

##### 冒泡排序

* 基本思想:

    > 在要排序的一组数中, 对当前还未排好序的范围内的全部数, 自上而下对相邻的两个数依次进行比较和调整, 让较大的数往下沉, 较小的往上冒.
    > 即:每当两相邻的数比较后发现它们的排序与排序要求相反时, 就将它们互换.

* 算法实现:

```python
def bubble_sort(seq):
    for i, item_i in enumerate(seq):
        for j, item_j in enumerate(seq):
            if item_j > item_i:
                seq[i], seq[j] = seq[j], seq[i]
    return seq

print bubble_sort([3, 6, 3, 44, 55, 11])
```

##### 快速排序

* 基本思想:

    > 通过一趟排序将要排序的数据分割成独立的两部分, 其中一部分的所有数据都比另外一部分的所有数据都要小, 
    > 然后再按此方法对这两部分数据分别进行快速排序, 整个排序过程可以递归进行，以此达到整个数据变成有序序列.

* 算法实现:

```python
"""
按照连表第一个数为参照排序
"""


def quick_sort(seq, start_i, end_j):
    start = start_i
    end = end_j
    if start >= end:
        return seq
    start_item = seq[start]
    while start < end:
        while start < end and seq[end] >= start_item:
            end -= 1
        seq[start] = seq[end]
        while start < end and seq[start] <= start_item:
            start += 1
        seq[end] = seq[start]
    seq[start] = start_item
    quick_sort(seq, start_i, start - 1)
    quick_sort(seq, end + 1, end_j)
    return seq

print quick_sort([3, 6, 3, 44, 55, 11], 0, 5)

"""
按照列表末尾的数为参照
"""


def quick_sort_end(seq, start_i, end_j):
    start = start_i
    end = end_j
    print seq
    if start >= end:
        return seq
    start_item = seq[end]
    while start < end:
        while start < end and seq[start] <= start_item:
            start += 1
        seq[end] = seq[start]
        while start < end and seq[end] >= start_item:
            end -= 1
        seq[start] = seq[end]
    seq[end] = start_item
    print seq, start, end, start_item, 'main'
    quick_sort_end(seq, start_i, start - 1)
    quick_sort_end(seq, end + 1, end_j)
    return seq

quick_sort_list = [924, 194, 830, 508, 936, 211, 187, 663, 220, 286]
print 'quick_end start'
print quick_sort_end(quick_sort_list, 0, len(quick_sort_list)-1)
print 'quick_end end'

```

##### 简单选择排序

* 基本思想: 
    > 选择排序也是一种简单直观的排序算法.它的工作原理很容易理解:首先在未排序序列中找到最小（大）元素, 存放到排序序列的起始位置;
    > 然后,再从剩余未排序元素中继续寻找最小（大）元素, 放到已排序序列的末尾. 以此类推，直到所有元素均排序完毕.

* 算法实现:

```python
def select_sort(seq):
    for i in xrange(len(seq)):
        min_item = seq[i]
        min_j = i
        for j, item in enumerate(seq[i:]):
            if item <= min_item:
                min_item = item
                min_j = j
        if min_j + i != i:
            seq[min_j + i], seq[i] = seq[i], seq[min_j + i]
    return seq

print select_sort([3, 6, 3, 44, 55, 11])


# 递归版本：
def select_sort_rec(seq, i):
    if i == 0: return 
    max_j = i
    for j in range(i):
        if seq[j] > seq[max_j]: max_j = j
    seq[i], seq[max_j] = seq[max_j], seq[i]
    select_sort_rec(seq, i-1)

```


##### 直接插入排序

* 基本思想:
    > 从第一个元素开始, 该元素可以认为已经被排序
    > 取出下一个元素, 在已经排序的元素序列中从后向前扫描, 找到已排序的元素小于或者等于新元素的位置, 将新元素插入到该位置后
    > 重复以上步骤, 直到排序完成

* 算法实现:

```python
def insert_sort(seq):
    for i in range(1, len(seq)):
        if seq[i - 1] > seq[i]:
            item = seq[i]
            index = i
            while index > 0 and seq[index - 1] > item:
                seq[index] = seq[index - 1]
                index -= 1
            seq[index] = item
    return seq

print insert_sort([3, 6, 3, 44, 55, 11])


# 递归版本
def insert_sort_rec(seq, i):
    if i == 0: return 
    insert_sort_rec(seq, i-1)
    j = i
    while j > 0 and seq[j-1] > seq[j]:
        seq[j-1], seq[j] = seq[j], seq[j-1]
        j -= 1

```

##### 希尔排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
...
```

##### 归并排序

* 基本思想:

* 时间复杂度: 
```math
O(nlg_n$)

$$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$$
\\(x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}\\)
```

* 算法实现:

```python
# coding: utf-8


def merge_sort(seq):
    mid = len(seq) / 2
    left, right = seq[:mid], seq[mid:]
    if len(left) > 1: left = merge_sort(left)
    if len(right) > 1: right = merge_sort(right)
    res = []
    while left and right:
        if left[-1] >= right[-1]:
            res.append(left.pop())
        else:
            res.append(right.pop())
    res.reverse()
    return (left or right) + res

desc_seq = [28, 12, 33, 22, 10, 39, 31, 9, 17]
print merge_sort(desc_seq)

```

##### 堆排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
...
```

##### 基数排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
...
```
