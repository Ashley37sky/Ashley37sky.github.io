---
title: F1_Sort
date: 2021-11-28 21:56:54
tags: 
categories: [数据结构和算法, 课程]
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

### Bubble Sort

```python
def BubbleSort(nums): # 两两比较，大的放后面
  n = len(nums)
  for i in range(n-1):
    # swap = 0
    for j in range(n-1-i):
      if nums[j] > nums[j+1]:
        nums[j+1], nums[j] = nums[j], nums[j+1]
        # swap = 1
    # if not swap:
      # break
  return nums
```

+ Worst O(n^2^); Best O(n)

### Selection Sort

```python
def SelectionSort(nums): # 每次选一个最小的放在前面
  n = len(nums)
  for i in range(n-1):
    min_index = i
    for j in range(i+1,n):
      if nums[j] < nums[min_index]:
        min_index = j
    nums[i], nums[min_index] = nums[min_index], nums[i]
  return nums
```

+ Worst O(n^2^); Best O(n^2^)
+ 应用：Top K elements; The Kth Largest

### Insertion Sort

```python
def InsertionSort(nums):  # 将第i个元素插入到[0~i]中合适的位置
  n = len(nums)
  for i in range(1, n):
    pos = i-1
    cur = nums[i]
    while pos>=0  and nums[pos] > cur:
      nums[pos+1] = nums[pos] # 位置要移动
      pos -= 1
    nums[pos+1] = cur
  return nums
```

+ Worst O(n^2^); Best O(n)

## Merge Sort

```python
def MergeSort(nums, left, right): # 存left, right避开了开空间
  if(left < right):
    mid = (left + right) // 2
    MergeSort(nums, left, mid)
    MergeSort(nums, mid + 1, right)
    Merge(nums, left, mid, right)
  return nums

def Merge(nums, left, mid, right):
  i = left
  j = mid + 1
  temp = []

  while i <= mid and j <= right:
    if nums[i] < nums[j]:
      temp.append(nums[i])
      i += 1
    else:
      temp.append(nums[j])
      j += 1

  while i<= mid:
    temp.append(nums[i])
    i += 1 
  while j<=right:
    temp.append(nums[j])
    j += 1

  nums[left:right+1] = temp
```

+ **没有worst case和best case，时间复杂度稳定是O(nlgn)，但是合并的时候需要开空间**

## Quick Sort

```python
def QuickSort(nums, left, right):
  if left < right:
      pivot = partition(nums, left, right)
      QuickSort(nums, left, pivot-1)
      QuickSort(nums, pivot+1, right)
  return nums

def partition(nums, left, right):
  pivot_val = nums[right]
  cur = left     
  for i in range(left, right):
    if nums[i] < pivot_val:
      nums[i], nums[cur] = nums[cur], nums[i]
      cur += 1
  nums[cur], nums[right] = nums[right], nums[cur]
  return cur
```

+ **Time: not stable. averge O(nlogn), worst scenario O(n^2).  但是省空间 O(1) space.**
+ 应用：the kTh largest element
+ 和 归并排序 比较
  + devide后就已经是顺序的，无需conquer
  + pivot选取随机，导致左右分布不均匀
  + Merge: stable O(nlogn). require O(n) space.
  + not stable. averge O(nlogn), worst scenario O(n^2). O(1) space.

### LC215 - Kth Largest Element in an Array

```python
class Solution(object):
    def findKthLargest(self, nums, k):
    
        n = len(nums)-1
        self.QuickSort(nums, 0, n, k-1)
        return nums[k-1]
        
    def QuickSort(self, nums, left, right, k):
        if(left <  right):
            pivot = self.Partition(nums, left, right)
            if pivot == k:
                return
            if pivot < k:
                self.QuickSort(nums, pivot+1, right, k)
            else:
                self.QuickSort(nums, left, pivot-1, k)
    
    def Partition(self, nums, left, right):
        pivot = nums[right]
        cur = left
        for i in range(left, right):
            if nums[i] > pivot:
                nums[i], nums[cur] = nums[cur], nums[i]
                cur += 1
        nums[cur], nums[right] = nums[right], nums[cur]
        return cur
        
```

+ 其他解法 Minheap

  ```python
  import heapq
  
  class Solution(object):
      def findKthLargest(self, nums, k):
          heap = nums[:k]
          heapq.heapify(heap)
  
          n = len(nums)
          for i in range(k, n):
              if nums[i] > heap[0]:
                  heapq.heappop(heap)
                  heapq.heappush(heap, nums[i])      
          return heap[0]
  ```
