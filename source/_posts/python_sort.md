layout: sort_layout
title: Python 实现八大排序
date: 2017-05-24 18:16:05
tags: 重点
---
### 排序算法

* 各种排序算法
![](https://martin-upload.b0.upaiyun.com/web/2017/05/4097d5d86145141fcb6a5ecff0fbc3bc.jpg)

* 各种排序算法比较
![](https://martin-upload.b0.upaiyun.com/web/2017/05/746fe11d81c35fc57bb2b8ba17be65c5.jpg)

##### 冒泡排序

* 基本思想:

    > 在要排序的一组数中，对当前还未排好序的范围内的全部数，自上而下对相邻的两个数依次进行比较和调整，让较大的数往下沉，较小的往上冒。
    > 即：每当两相邻的数比较后发现它们的排序与排序要求相反时，就将它们互换。

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

    > 通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，
    > 然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

* 算法实现:

```python
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
```

##### 简单选择排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

##### 堆排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

##### 直接插入排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

##### 希尔排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

##### 归并排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

##### 基数排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```
