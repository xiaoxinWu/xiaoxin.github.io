---
layout:     post
title:      代码随想录DAY2
subtitle:   leetcode刷题笔记
date:       2023-04-06
author:     xiaoxin
header-img: img/post-bg-debug.png
catalog: true
tags:
    - 代码随想录
    - leetcode
---

> Task：977.有序数组的平方 ，209.长度最小的子数组 ，59.螺旋矩阵II ，数组总结

- [x] 977.有序数组的平方
- [x] 209.长度最小的子数组
- [x] 59.螺旋矩阵II
- [x] 数组总结

### 977.有序数组的平方
[题目链接](https://leetcode.cn/problems/squares-of-a-sorted-array/)
[文章讲解](https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html)

- 解题思路：
> 因为数组有负值，看到题目的第一反应是使用双指针，从两端计算大的数据，从尾部开始更新结果数组

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        ## 双指针
        res = [-1] * len(nums)
        left, right = 0, len(nums)-1
        for i in range(len(nums)-1, -1, -1):
            if nums[left] * nums[left] < nums[right] * nums[right]:
                res[i] = nums[right] * nums[right]
                right -= 1
            else:
                res[i] = nums[left] * nums[left]
                left += 1
        return res

```

- 难点：
	主要是想到使用双指针，否则平方后再排序很容易超时

- 收获：
	学习了双指针的使用，对于降低时间复杂度十分有效

### 209.长度最小的子数组
[题目链接](https://leetcode.cn/problems/minimum-size-subarray-sum/)
[文章讲解](https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html)
- 解题思路：
> 解题的想法是双指针/滑动窗口，根据窗口的数值和来更新窗口的两端

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        """
        思路：使用滑动窗口，得到每个窗口的sum
        当sum<target 右移 更新sum
        当sum>target 左移 更新sum
        """
        if sum(nums) < target: return 0
        subSum = 0
        minLen = len(nums) + 1
        i = 0
        for j in range(len(nums)):
            subSum += nums[j]
            while subSum >= target:
                minLen = min(minLen, j-i+1)
                subSum -= nums[i]
                i += 1
        return minLen
```

- 难点：
难点在于滑动窗口的定义和窗口左右边界如何进行更新，之后还有定长的滑动窗口，左右边界同时更新，之后再总结。

- 收获：
学习了滑动窗口的题目，之后需要继续精进。

### 59.螺旋矩阵II
[题目链接](https://leetcode.cn/problems/spiral-matrix-ii/)
[文章讲解](https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html)

- 解题思路：
> 这道题就算典型的模拟题，需要更新每次模拟的边界和需要更新的数据点的坐标
> 最后对奇偶的n需要重新更新中心点位置的数据

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        """
        思路：使用左闭右开进行数组访问，模拟顺时针矩阵的生成
        主要考察的是写代码的能力
        """
        mat = [[0] * n for _ in range(n)]   ## 输出数组
        startX, startY = 0, 0    ## 每次循环的起始点
        loop, mid = n // 2, n // 2  ## 循环圈数 中点位置，奇数n需要直接设置该点数字
        count = 1
        # 每循环一层偏移量加1，偏移量从1开始
        for offset in range(1, loop+1): 
            ### 左到右 [startY, n-offset]
            for i in range(startY, n-offset):
                mat[startX][i] = count
                count += 1
            ### 从上往下 [startX, n-offset]
            for i in range(startX, n-offset):
                mat[i][n-offset] = count
                count += 1
            ## 从右至左
            for i in range(n-offset, startY, -1):
                mat[n-offset][i] = count
                count += 1
            ## 从下至上
            for i in range(n-offset, startX, -1):
                mat[i][startY] = count
                count += 1
            startX += 1
            startY += 1
        if n%2 == 1:
            mat[mid][mid] = count
        return mat
```

- 难点：

	偏移量的更新
> 偏移量和loop有关，需要注意

	遍历时待更新点的坐标
> 坐标很容易搞混，需要手动模拟一下，同时一次循环后，需要更新X和Y的起始点坐标

- 收获
	学习了数组模拟访问的问题，之后n皇后问题和机器人遍历数组都需要相关的代码能力

### 数组总结

#### 二分法

1.  二分的使用前提：有序数组并且无重复元素。
2.  分清左闭右闭和左闭右开的不同情况。

-   左闭右闭

> right = nums.length - 1 while (left <= right) right = mid - 1; left = mid + 1;

-   左闭右开

> right = nums.length; while (left < right) right = mid; left = mid + 1;

#### 双指针法

通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。

#### 滑动窗口

原理和双指针一样。两个指针都能固定住，并且一个指针固定，另一个指针不断移动，从而得出我们要想的结果。

#### 螺旋矩阵

旋转时，保持每一边一样的处理规则，比如左闭右开，或者左开右闭。

对于正方形的二维教组只需要考虑转多少圈以及n为基数时中间剩一个值，如果剩一个一个值需要再转完圈后再单独加中间的值。













