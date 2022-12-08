---
title: 09-LeetCode
tags:
  - LeetCode
cover: >-
  https://github.com/Monaxi/Mona-study/blob/main/blog/wallhaven-wqgr9x_1920x1080.png?raw=true
top_img: >-
  https://github.com/Monaxi/Mona-study/blob/main/blog/default_top_img.png?raw=true
date: 2022-06-27 22:37:22
description:
---

# 数据结构入门

## 217. 存在重复元素

**难度** <font color=green>简单</font>

### 题目描述

给你一个整数数组 `nums` 。如果任一值在数组中出现**至少两次**，返回 `true` ；如果数组中每个元素互不相同，返回 `false` 。

**示例 1：**

```
输入：nums = [1,2,3,1]
输出：true
```

**示例 2：**

```
输入：nums = [1,2,3,4]
输出：false
```

**示例 3：**

```
输入：nums = [1,1,1,3,3,4,3,2,4,2]
输出：true
```

**提示：**

- 1 <= nums.length <= 105
- -109 <= nums[i] <= 109

### 解题思路

**方法一、排序**

```python
class Solution:
    def containsDuplicate(self, nums) :
        if len(nums) <= 1:
            return False
        nums.sort()
        for i in range(0, len(nums)-1):
            if nums[i] == nums[i+1]:
                return True
        return False
```

**方法二、哈希表**

将数组中的每个元素插入哈希表中，如果插入一个元素时发现哈希表中已经存在该元素，则说明有重复元素；

在python中也可以理解为，将每个元素插入到字典中，遍历字典，使用get函数找该num是否出现过（？）

get函数用法：get(key, value)，其中value可选，value是当指定的键值不存在的时候返回的值；

```python 
class Solution(object):
  	def containsDuplicate(self, nums):
      	record = dict()
        for num in nums:
          	if record.get(num, 0):
              	return True
            record[num] = 1
        return False
```

**方法三、看长度是否有变化**

将数组转集合，看看长度是否有变化；

`set()`函数创建一个无序不重复的元素集，可进行关系测试，删除重复数据，还可以计算交集、差集、并集等；

```python
class Solution(object):
  	def containsDuplicate(self, nums):
      	return len(nums) != len(set(nums))
```



## 52. 最大子数组和

**难度**：<font color=green>简单</font>

### 题目描述

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

**提示：**

- 1 <= nums.length <= 105
- -104 <= nums[i] <= 104



**进阶：**如果你已经实现复杂度为 `O(n)` 的解法，尝试使用更为精妙的 **分治法** 求解。
