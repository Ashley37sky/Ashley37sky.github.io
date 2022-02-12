---
title: F1_搜索
date: 2021-11-29 22:49:53
tags: 
categories: 数据结构和算法
---

# F1_搜索

> LC912 排序

## Basic

+ bubble sort: 遍历n-1次，每一次两两比较swap
  + O(n^2)  -- O(n)
+ selection sort: 遍历n-1次，每一次找最小放在前面
  + O(n^2)  -- O(n)

<!--more-->

+ insertion sort: 从头到尾遍历，每一次将第i个元素放到[0, i-1]中合适的位置
  + O(n^2)  -- O(n)

## merge sort

+ time Complexity: `O(n*log n)`
  + divide: logn
  + conquer: n （two sorted --> one sorted 只需要遍历一遍）
+ space O(n)

```python
class Solution(object):
    def sortArray(self, nums):
        
        if len(nums) > 1: # 若只有一个元素 直接返回    
            mid = len(nums) // 2
            l = nums[:mid]
            r = nums[mid:]
            
            self.sortArray(l)
            self.sortArray(r)
            
            i = j = k = 0
            while i < len(l) and j < len(r): # 两个sorted进行排序
                if l[i] < r[j]:
                    nums[k] = l[i] # 直接利用nums不开新空间
                    i += 1
                else:
                    nums[k] = r[j]
                    j += 1
                k += 1
                
            while i < len(l): # 将剩下的全部放在后面
                nums[k] = l[i]
                i += 1 
                k += 1
            while j < len(r):
                nums[k] = r[j]
                j += 1
                k += 1
                
        return nums
        
```

## quick sort

+ 算法
  + 选取pivot
  + 小于放在pivot左边，大于放右边
  + 左边/右边repeat

+ time: O(nlogn) ~ O(n^2)
+ space: O(1)
+ 和 归并排序 比较
  + devide后就已经是顺序的，无需conquer
  + pivot选取随机，导致左右分布不均匀
  + Merge: stable O(nlogn). require O(n) space.
  + not stable. averge O(nlogn), worst scenario O(n^2). O(1) space.

