---
layout: post
title: 2016 小米 校园招聘 笔试题
---
## 线段覆盖
一条直线上有 `N` 条线段，已知它们的两个端点，计算这些线段覆盖了多大的长度（被多条线段覆盖的部分只计算一次）

### 算法
将线段打散成点，以处理重复的部分

### Python代码实现

```python

class Segment():
    def __init__(self, start, end):
        self.start = start
        self.end = end

def Sum(segments):
    coverd = set()
    for segment in segments:
        for i in range(segment.start, segment.end):
            coverd.add(i)
    return len(coverd)

if __name__ == '__main__':
    segments = [Segment(1, 3), Segment(2, 5), Segment(10, 15)]
    print Sum(segments)

```

## 移位数组
给定一个整数数组 `A` 和一整数 `key` ，已知这个数组是由一个已排序、递增的数组旋转移位而成，但现在不知道这个数组被移动了多少位。<br>
请实现下面的函数，找出 `key` 在 `A` 中的位置。<br>
最后请分析所写代码的时间、空间复杂度，评分会参考代码的正确性和效率。<br>
例如：<br>
`A = [6, 7, 8, 5] // 由[5, 6, 7, 8]向右移动3位而成`<br>
`n = 8`<br>
结果为`2`

### 算法
二分查找。总是在有序的那一半里比较`key`，如果在这一半中，就在这半部分里继续二分查找，如果不在这半部分里，就舍弃它，在另一半里重复上面的操作

### Python代码实现

```python

def rotatedBinarySearch(A, N, key):
    L = 0
    R = N - 1

    while L <= R:
        # 避免溢出, 和 M = (L + R) / 2 是一样的
        M = L + ((R - L) / 2)
        if A[M] == key:
            return M

        # 左半部分有序
        if A[L] <= A[M]:
            if A[L] <= key and key < A[M]:
                R = M - 1
            else:
                L = M + 1

        # 右半部分有序
        else:
            if A[M] < key and key <= A[R]:
                L = M + 1
            else:
                R = M - 1

    return -1

if __name__ == '__main__':
    A = [6, 7, 8, 5]
    N = len(A)
    key = 8    
    print rotatedBinarySearch(A, N, key) # 程序输出 2

```

时间复杂度为 `O(2logN)`

## 朋友圈
假如已知有 `n` 个人和 `m` 个好友关系( `r` )。如果两个人是直接或间接的好友（好友的好友的好友...），则认为他们属于同一个朋友圈。请写程序求出这n个人里一共有多少个朋友圈。<br>
例如： `n = 5, m = 3, r = [[1, 2], [2, 3], [4, 5]]` , 表示有5个人，1和2是好友，2和3是好友，4和5是好友，则1、2、3属于一个朋友圈，4、5属于另一个朋友圈。结果为2个朋友圈。<br>
最后请分析所写代码的时间、空间复杂度。评分会参考代码的正确性和效率。

### 算法
用并查集，具体可以看[这篇博客](http://blog.csdn.net/dm_vincent/article/details/7655764)

### Python代码实现

```python
class UnionFind():
    def __init__(self, n):
        self.id = range(1, n + 1)

    def find(self, p):
        return self.id[p - 1]

    def union(self, p, q):
        pID = self.find(p)
        qID = self.find(q)
        if pID == qID:
            return
        else:
            for i in range(len(self.id)):
                if self.id[i] == pID:
                    self.id[i] = qID

if __name__ == '__main__':
    n = 5
    m = 3
    r = [[1, 2], [2, 3], [4, 5]]
    UF = UnionFind(n)
    for relation in r:
        UF.union(relation[0], relation[1])
    count = set()
    for i in UF.id:
        print i
        count.add(i)
    print len(count) # 程序输出2
```
