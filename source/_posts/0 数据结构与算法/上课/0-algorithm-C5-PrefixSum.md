---
title: C5-Prefix Sum
date: 2022-02-18 12:32:32
tags:
categories: [数据结构和算法, 课程]
---

# Prefix Sum

## 分类

- Array
- Matrix

<!--more-->

## 特点

- 解法固定, 套用模板
- 关键词: **continuous subarray**, consecutive elements,  consecutive sequence, Subarray Sum

## 技巧

- 记录什么?  和对应的公式
- 起点 （初始化什么）


## Problems

- 典型:  560, 523

- Matrix: 304

- （还没做）练习题: 128, 1248, 974, 325,  Matrix: 1314

- （还没做）剑指: 66(题不好, 非常规prefix sum). 

# 练习

## LC 560 - Subarray Sum Equals K

+ Given an array of integers `nums` and an integer `k`, return *the total number of subarrays whose sum equals to `k`*

+ key:  
  + Hashmap: dic[prefix_sum] = number of arrays, 存满足sum的array数量，不用存index（与position无关）
  + 初始值 {0:1}

```python
class Solution(object):
    def subarraySum(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        count = 0
        sums = 0
        
        dic = {0:-1} # dic[prefix_sum] = number of arrays
        
        for i in range(len(nums)):
            sums += nums[i]
            count += dic.get(sums-k,0) # # dict.get(key, default=None) 默认返回值
            dic[sums] = dic.get(sums,0) + 1
        
        return(count)
```

## LC 523 - Continuous Subarray Sum

+ Given an integer array `nums` and an integer `k`, return `true` *if* `nums` *has a continuous subarray of size **at least two** whose elements sum up to a multiple of* `k`*, or* `false` *otherwise*.
  +  return `true`, 不用返回count，不用存numbers； **at least two**，需要存储index --> dic[prefix_sum] = index
  + sum up to a multiple of* `k` : nums[i] % k == nums[j] %k --> nums[j] - nums[i] = n*k
+ key: 初始值 dic[prefix_sum] = index {0, -1}

```python
class Solution(object):
    def checkSubarraySum(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        """
        
        sums = 0
        dic = {0:-1} # dic[prefix_sum] = index
        
        for i in range(len(nums)):
            
            sums = (sums + nums[i]) % k
            
            if sums not in dic:
                dic[sums] = i
            else:
                if i - dic[sums] >= 2:
                    return True
        return False
```

## LC 304 - Range Sum Query 2D

+ Given a 2D matrix `matrix`, handle multiple queries of the following type:
  - Calculate the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.
+ key: 矩阵和，数学解法
  + 注意扩展上边和左边为0

```python
class NumMatrix(object):

    def __init__(self, matrix):
        """
        :type matrix: List[List[int]]
        """
        row, col = len(matrix), len(matrix[0])
            
        self.sums = [ [0] * (col + 1) for _ in range(row + 1) ] # 上和左要扩展一圈
        
        for i in range(1, row+1):
            for j in range(1, col+1):
                self.sums[i][j] = self.sums[i][j-1] + self.sums[i-1][j] - self.sums[i-1][j-1] + matrix[i-1][j-1]
        

    def sumRegion(self, row1, col1, row2, col2):
        return self.sums[row2 + 1][col2 + 1] - self.sums[row2 + 1][col1]-self.sums[row1][col2 + 1] + self.sums[row1][col1]
```
