---
layout: post
title: Count Primes
---
[LeetCode](https://leetcode.com)上面有一道数质数的题（[Count Primes](https://leetcode.com/problems/count-primes/)），里面给的提示是一种非常快的算法，感觉很经典，现在我把它的提示翻译出来，并附上自己的Python代码。

1. 先从 *isPrime* 函数开始说起。为确定一个数是否是质数，要检查它是否能被 *n* 以下的整数整除。所以 *isPrime* 的时间复杂度是O(*n* ), 而数质数的时间复杂度就是O(*n2*)。还能做到更好吗？

2. 我们都知道这个数不能被 > *n* / 2 的数整除，所以我们可以把迭代次数砍半。还能做到更好吗？

3. 先写一下12的约数：<br>
2 × 6 = 12<br>
3 × 4 = 12<br>
4 × 3 = 12<br>
6 × 2 = 12<br>
可以看出，没必要计算 4 × 3 和 6 × 2 。所以，只要计算到 *√n* 就可以了。因为如果 *n* 可以被 *p* 整除，那么 *n* = *p* × *q*, 又 *p* ≤ *q*，那么可以推出 *p* ≤ *√n* 。现在的时间复杂度达到了O(*n1.5*)。还有更快的办法吗？

4. 埃拉托色尼筛选法([Sieve of Eratosthenes](http://en.wikipedia.org/wiki/Sieve_of_Eratosthenes))是找质数最快的的算法。别看名字很吓人，概念其实很简单。<br>
![埃拉托色尼筛选法](http://prdwb.github.io/images/2015/10/Sieve_of_Eratosthenes_animation.gif)<br>
这是一个 *n* 个数的表格。先看第一个数字，2。2的倍数显然不是质数，先把它们划掉。再看下一个数，3，把它的倍数也划掉。再看下一个数，4，它已经被划掉了。这告诉了你什么？我们还需要划掉4的倍数吗？

5. 4不是质数，因为它可以被2整除，这就意味着所有4的倍数都能被2整除，而且已经被划掉了。所以我们可以跳过4。再看下一个数，5，它的倍数也可以被划掉。这里还有优化的空间，我们不需要从 5 × 2 = 10 开始，那么该从哪开始呢？

6. 其实，我们可以从 5 × 5 = 25 开始。因为 5 × 2 = 10 在划2的倍数时就已经被划掉了，相似的，5 × 3 = 15 在划3的倍数时就已经被划掉了。所以，如果当前数字是 *p* ，我们可以从 *p2* 开始划，接着划*p2* + *p*, *p2* + *2p*, ...那么循环结束的条件是什么呢？

7. 用 *p* < *n* 来结束循环是最简单的想法，但它效率不高。你还记得第3点吗？

8. 是的，可以用 *p* < *√n* 来结束循环，因为所有 ≥ *√n* 的非质数已经被划掉了。等循环结束时，表格中还没被划掉的数就是质数。

下面附上以上算法的Python代码实现:

```python
import math
class Solution(object):
    def countPrimes(self, n):
        """
        :type n: int
        :rtype: int
        """
        isPrime = [False, False] //把0,1置成非质数
        for i in range(2, n):
            isPrime.append(True)
        k = int(math.ceil(math.sqrt(n)))
        for i in range(2, k):
            if not isPrime[i]:
                continue
            for j in range(i * i, n, i):
                isPrime[j] = False
        count = 0
        for i in range(2, n):
            if isPrime[i]:
                count += 1
        return count
```