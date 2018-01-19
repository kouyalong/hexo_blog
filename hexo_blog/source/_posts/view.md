layout: review_layout
title: 知识点总结
date: 2018-01-18 15:50:05
tags: 重点
comments: true
categories: Structure

---

### TCP UDP
通信技术大致分为三类：面向连接的电路交换，面向连接的包交换，以及无连接的包交换。

网络层的分组传输是不可靠的，无法了解数据到达终点的时间，无法了解数据未到达终点的状态，传输层增强了网络层的服务质量

名称 | 过程 | 传输数据单元 | 优点 | 缺点 | 应用 |
---|---|---|---|---|---|
面向连接 | 1.建立连接 2.数据传输 3.释放链接 | 段（segment）| 可确保数据传送的次序和传输的可靠性 | 传输数据相对较大，建立连接断开连接的时间和数据传输开销 | HTTP、FTP
无连接 | 1.传输数据（把分组传输到目的地；分组丢失，接收方监测之后请求重发）|  数据报（datagram） | 灵活方便、迅速，特别适合于传送少量零星的报文 | 不能防止报文的丢失、重复或失序 | DNS、即时聊天 |

##### TCP
###### TCP 三次握手
![](http://dl.wcar.net.cn/jpg/2018/1/10//Users/kouyalong/Pictures/20180110113301.jpg)
###### TCP状态转移图
![](http://dl.wcar.net.cn/jpg/2018/1/10//Users/kouyalong/Pictures/544465b00200001s.png)
##### 引用
- [浅析tcp（中）](https://zhuanlan.zhihu.com/p/22095090)
- [再叙TIME_WAIT](https://huoding.com/2013/12/31/316)
- [TCP系列01—概述及协议头格式](https://www.cnblogs.com/lshs/p/6038458.html)
- [TCP、UDP详解](https://www.cnblogs.com/visily/archive/2013/03/15/2961190.html)
### HTTP协议报文
#### cookie session
### Mysql两种引擎
#### Innodb索引
#### Innodb事物
#### mysql主存同步
### Redis
### LRU算法
### 二叉树
### 堆

```python
# -*- coding: utf-8 -*-


class SortingAlgorithms(object):

    def max_heapify(self, heap, heap_size, root):
        # 在堆中做结构调整使得父节点的值大于子节点

        left, right = 2 * root + 1, 2 * root + 2
        larger = root
        if left < heap_size and heap[larger] < heap[left]:
            larger = left
        if right < heap_size and heap[larger] < heap[right]:
            larger = right
        if larger != root:
            # 如果做了堆调整则larger的值等于左节点或者右节点的，这个时候做对调值操作
            heap[larger], heap[root] = heap[root], heap[larger]
            self.max_heapify(heap, heap_size, larger)

    def min_heapify(self, heap, heap_size, root):
        left, right = 2 * root + 1, 2 * root + 2
        little = root
        if left < heap_size and heap[little] > heap[left]:
            little = left
        if right < heap_size and heap[little] > heap[right]:
            little = right
        if little != root:
            heap[little], heap[root] = heap[root], heap[little]
            self.min_heapify(heap, heap_size, little)

    def make_heapify(self, heap, heap_size, root, max_heap=False):
        if max_heap:
            self.max_heapify(heap, heap_size, root)
        else:
            self.min_heapify(heap, heap_size, root)

    def build_heap(self, heap, max_heap=False):
        # 构造一个堆，将堆中所有数据重新排序
        heap_size = len(heap)
        # 将堆的长度单独计算出来
        for i in xrange((heap_size - 2) // 2, -1, -1):
            # 从后往前出数
            self.make_heapify(heap, heap_size, i, max_heap=max_heap)

    def heap_sort(self, heap, max_heap=False):
        # 将根节点取出与最后一位做对调，对前面len-1个节点继续进行对调整过程。
        self.build_heap(heap, max_heap=max_heap)
        print heap
        for i in range(len(heap) - 1, -1, -1):
            heap[0], heap[i] = heap[i], heap[0]
            self.make_heapify(heap, i, 0, max_heap=max_heap)
        return heap
```

### 链表
### 设计模式
### python基础
#### 协程
#### 装饰器
#### GC
#### 元类（metaclass）
### RESTful API
### 排序算法
### 二分思想
### 多路归并

```python
#coding: utf-8

def merging_sorting(data):
    """
    data = [[11, 34, 56], [6, 23, 57], [10, 19, 33]]
    """
    # 生成一个归并序列，把每一路中的元素挨个排入compare数组中
    k = len(data)
    compare = []
    for i in xrange(0, k):
        compare.append(data[i][0])
        # data[i].remove(data[i][0])
    print compare

    def adjust(loserTree, dataArray, n, s):
        # 败者树的核心代码
        t = (s + n) / 2
        while t > 0:  # 从败者树的尾部开始进行比较
            if dataArray[s] > dataArray[loserTree[t]]:
                # 和败者结点比较
                s, loserTree[t] = loserTree[t], s
                # 如果比某个败者结点大，说明该结点失败了，将s结点存入败者树，把败者树的现在的胜结点拿去和其父节点比较。
            t /= 2
        loserTree[0] = s

    def createLoserTree(loserTree, dataArray, n):
        for i in range(n):
            loserTree.append(0)
            dataArray.append(i-n)
            # 这里是为了生成败者树用的，

        for i in range(n):
            print loserTree, dataArray, n, n-1-i
            adjust(loserTree, dataArray, n, n-1-i)

    loserTree = []
    dataArray = []
    createLoserTree(loserTree, dataArray, k)
    print loserTree, dataArray

    for i in xrange(k):
        dataArray[i] = compare[i]
        # 将数据替换成待排数据
        adjust(loserTree, dataArray, k, i)
        # 此步执行完毕，败者树才完全创建初始化完毕，正式开始排序归并
    print loserTree, dataArray
    result = []
    while True:
        if data[loserTree[0]][0] > 10000:
            break
        result.append(data[loserTree[0]][0])
        # 添加到 result 中，这是我们需要的结果
        data[loserTree[0]].remove(data[loserTree[0]][0])
        # 从data 中删除头一个元素
        if data[loserTree[0]] is not []:
            data[loserTree[0]].append(10001)
        compare[loserTree[0]] = data[loserTree[0]][0]
        # 将下一个元素添加到 compare数组中
        print loserTree, compare, data
        adjust(loserTree, compare, k, loserTree[0])
```
