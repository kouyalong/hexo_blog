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

#### 冒泡排序

* 基本思想: 

* 时间复杂度:

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

#### 快速排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

#### 简单选择排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

#### 堆排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

#### 直接插入排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

#### 希尔排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

#### 归并排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```

#### 基数排序

* 基本思想:

* 时间复杂度:

* 算法实现:

```
```
