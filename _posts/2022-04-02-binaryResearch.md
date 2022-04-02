---
layout: post
title: 二分查找
date: 2022-04-02
tags: 算法
---

34 69 367

## [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

### 题目

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

示例 1:
> 输入: nums = [1,3,5,6], target = 5
> 输出: 2

示例 2:
> 输入: nums = [1,3,5,6], target = 2
> 输出: 1

示例 3:
> 输入: nums = [1,3,5,6], target = 7
输出: 4

### 解法

首先定义target所在区间是左闭右闭的，即[left, right]。

所以：

+ while中的条件是(left <= right)，而不是left < right；
+ 当nums[mid] != target而给left或right重新赋值时，应赋mid + 1或mid - 1。

代码如下：

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        int mid;
        while(left <= right)
        {
            mid = left + (right - left) / 2; // 防止溢出,等效于(left + right)/2
            if(nums[mid] < target)
            {
                left = mid + 1; // 新的区间为[mid + 1, right]
            }
            else if(nums[mid] > target)
            {
                right = mid - 1; // 新的区间为[left, mid - 1]
            }
            else if(nums[mid] == target)
                return mid;
        }
        return left;
    }
};
```

检验一下所有可能的输入：

1. target等于其中一个值，if(nums[mid] == target)	return mid，显然正确；
2. target比所有值都小，应该插到最左边：此时最后一次进循环是left = right = 0，执行if(nums[mid] > target)	right = mid - 1; return left; 即返回0，确实插到了最左边；
3. target比所有值都大，应该插到最右边：此时最后一次进循环是left = right =nums.size() - 1，执行if(nums[mid] < target)	left = mid + 1;  return left; 即返回right =nums.size()，确实插到了最右边；
4. target夹在两个值之间，left仍能指向比target值大的最小数，也正确插入。

时间复杂度：O(log n)

空间复杂度：O(1)

## [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

### 题目

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。


示例 1:
>输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4

示例 2:
>输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1

提示：
你可以假设 nums 中的所有元素是不重复的。
n 将在 [1, 10000]之间。
nums 的每个元素都将在 [-9999, 9999]之间。

### 解法

把上面那题的left改成-1就行了。

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        int mid;
        while(left <= right)
        {
            mid = left + (right - left) / 2; // 防止溢出,等效于(left + right)/2
            if(nums[mid] < target)
            {
                left = mid + 1; // 新的区间为[mid + 1, right]
            }
            else if(nums[mid] > target)
            {
                right = mid - 1; // 新的区间为[left, mid - 1]
            }
            else if(nums[mid] == target)
                return mid;
        }
        return -1;
    }
};
```

时间复杂度：O(log n)

空间复杂度：O(1)

## [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

### 题目

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

进阶：

你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？

示例 1：
>输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]

示例 2：
>输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]

示例 3：
>输入：nums = [], target = 0
输出：[-1,-1]

提示：
0 <= nums.length <= 105
-109 <= nums[i] <= 109
nums 是一个非递减数组
-109 <= target <= 109

### 解法



## [69. x 的平方根 ](https://leetcode-cn.com/problems/sqrtx/)

### 题目

给你一个非负整数 x ，计算并返回 x 的 算术平方根 。

由于返回类型是整数，结果只保留 整数部分 ，小数部分将被 舍去 。

注意：不允许使用任何内置指数函数和算符，例如 pow(x, 0.5) 或者 x ** 0.5 。


示例 1：
>输入：x = 4
输出：2

示例 2：
>输入：x = 8
输出：2
解释：8 的算术平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去。

提示：
0 <= x <= 231 - 1

### 解法

