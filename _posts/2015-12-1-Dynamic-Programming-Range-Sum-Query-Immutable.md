---
layout: post
title: 动态规划：Range Sum Query - Immutable
---
## 动态规划 (Dynamic Programming)
动态规划类的问题有两个突出的属性：<br>
* Overlapping subproblems: 可以分解为若干个子问题，并且解决这些子问题的方法有重复性<br>
* Memoization: 可以反复应用以前的结果，无需再次计算

## Range Sum Query - Immutable
[Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) 是 [LeetCode OJ](https://leetcode.com/) 上的一道题，它能很好的展现上面两个思想。

> Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.<br>
>
> Example: <br>
> Given nums = [-2, 0, 3, -5, 2, -1]
>
> sumRange(0, 2) -> 1<br>
> sumRange(2, 5) -> -1<br>
> sumRange(0, 5) -> -3<br>
>
>Note:<br>
>You may assume that the array does not change.<br>
>There are many calls to sumRange function.

### 算法
>There are many calls to sumRange function.<br>

这句提示就决定了这是一个动态规划问题。<br>
定义一个数组 `partialSum` , 将从第 `0` 到第 `j` 项的和存在 `partialSum[j]` 中，以便以后直接调用，无需再次求和。

### Java

```java

public class NumArray {
    private int[] nums;
    private ArrayList<Integer> partialSum = new ArrayList();

    public NumArray(int[] nums) {
        this.nums = nums;
        getPartialSum();
    }

    private void getPartialSum() {
        if(nums.length > 0)
            partialSum.add(nums[0]);
        for(int i = 1; i < nums.length; i++)
            partialSum.add(partialSum.get(i - 1) + nums[i]);
    }

    public int sumRange(int i, int j) {
        int sum = partialSum.get(j);
        if(i > 0)
            sum -= partialSum.get(i - 1);
        return sum;
    }
}


// Your NumArray object will be instantiated and called as such:
// NumArray numArray = new NumArray(nums);
// numArray.sumRange(0, 1);
// numArray.sumRange(1, 2);

```

### Python

```python

class NumArray(object):
    def __init__(self, nums):
        """
        initialize your data structure here.
        :type nums: List[int]
        """
        self.nums = nums
        self.partialSum = []
        self.getPartialSum()

    def getPartialSum(self):
        if len(self.nums) > 0:
            self.partialSum.append(self.nums[0])
        for i in range(1, len(self.nums)):
            self.partialSum.append(self.partialSum[i - 1] + self.nums[i])

    def sumRange(self, i, j):
        """
        sum of elements nums[i..j], inclusive.
        :type i: int
        :type j: int
        :rtype: int
        """
        sum = self.partialSum[j]
        if i >= 1:
            sum -= self.partialSum[i - 1]
        return sum


# Your NumArray object will be instantiated and called as such:
# numArray = NumArray(nums)
# numArray.sumRange(0, 1)
# numArray.sumRange(1, 2)

```
