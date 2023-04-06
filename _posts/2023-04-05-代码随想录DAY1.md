---
layout:     post
title:      代码随想录DAY1
subtitle:   leetcode刷题笔记
date:       2023-04-05
author:     xiaoxin
header-img: img/post-bg-debug.png
catalog: true
tags:
    - 代码随想录
    - leetcode
---

> Task: 数组理论基础，704. 二分查找，27. 移除元素

- [x] 数组理论基础
- [x] 704.二分查找
- [x] 34.在排序数组中查找元素的第一个和最后一个位置
- [x] 35.搜索插入位置
- [x] 27.移除元素

### 1.数组理论基础

**数组是存放在连续内存空间上的相同类型数据的集合。**
**数组的元素是不能删的，只能覆盖。**

### 704.二分查找
[题目链接](https://leetcode.cn/problems/binary-search)
[文章讲解](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html#_704-%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE)
解题思路：
-   该题使用二分法解答。即从中间开始查找，根据结果的大小再向左或者向右查找。可以立刻想到用left、right指针。
-   注意使用二分的原因：该数组为**升序**。
-   注意区间问题，判断条件是否能遍历所有下标。

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        """
        思路：由于数组为升序，使用双指针，从两头开始查找目标元素，区间左闭右开
        """
        left, right = 0, len(nums)-1
        while left <= right:
            mid = left + ((right - left)>>1)
            if target > nums[mid]:
                left = mid + 1
            elif target < nums[mid]:
                right = mid - 1
            else:
                return mid
        return -1
```

难点：
- 二分法区间问题
> 写二分法，区间的定义一般为两种，左闭右闭即[left, right]，或者左闭右开即[left, right)。

第一种写法，定义 target 是在一个在左闭右闭的区间里，**也就是[left, right] （这个很重要非常重要）**。

	while (left <= right) 要使用 <= ，因为left == right是有意义的，所以使用 <=
	if (nums[middle] > target) right 要赋值为 middle - 1，因为当前这个nums[middle]一定不是target，那么接下来要查找的左区间结束下标位置就是 middle - 1

- 关于mid的写法
	`mid = left + (right - left) // 2` 可以防止int类型上溢，同时除以2可以改成 `>>1`

收获：

这道题刚开始以为是简单，随便就能做；但是考虑区间问题和mid的耗时，细节点很多。

### 27.移除元素

[题目链接](https://leetcode.cn/problems/remove-element)
[文章讲解](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html#_27-%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0)
解题思路：

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        """
        思路：第一反应是使用双指针处理，并且指针从左右两边进行访问
        当nums[right]==val时，right-=1
        当nums[left]==val时，left和right的val互换，且left+=1 right-=1
        否则 left右移一个位置
        当left>right时跳出循环，返回left当作长度
        Args:
            nums (List[int]): 数组
            val (int): 待移除的值

        Returns:
            int: 移除元素后数组的新长度
        """
        left, right = 0, len(nums)-1
        while left <= right:
            if nums[right] == val:
                right -= 1
            elif nums[left] == val:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
                right -= 1
            else:
                left += 1
        return left
```

难点：
难点在什么时候跳出循环，这里使用了左闭右开的方案。
第二个难点在如果左右指针都不为val时，左指针需要右移，否则会进入死循环。

收获：
这道题熟悉了双指针的解法。

### 34. 在排序数组中查找元素的第一个和最后一个位置
[题目链接](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/solution/)
[文章讲解](无)
解题思路：

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        """
        思路：可以使用两遍二分查找得到左右边界
        1.得到左边界
        2.得到右边界
        Args:
            nums (List[int]): 排序数组
            target (int): 目标值

        Returns:
            List[int]: [start， end]
        """
        left, right = 0, len(nums)
        if not nums: return [-1, -1]
        res = []
        while left < right:
            mid = left + ((right - left) >> 1)
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
            else:
                right = mid
        if left == len(nums): return [-1, -1]
        if nums[left] ==  target:
            res.append(left)
        else:
            return [-1, -1]
        
        left, right = 0, len(nums)
        while left < right:
            mid = left + ((right - left) >> 1)
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
            else:
                left = mid + 1
        if right == 0 and nums[right] != target:
            return [-1, -1]
        res.append(right-1)
        return res
```

难点：
本题需要进行两遍二分查找，得到左右边界
难点主要是左右边界的判断，排除异常值
使用的是左闭右闭的方法去查找边界（左闭右开会找不到右边界）

收获：
熟悉了两种二分查找。












