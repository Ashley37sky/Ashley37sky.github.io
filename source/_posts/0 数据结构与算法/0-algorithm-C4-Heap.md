---
title: C4-Heap
date: 2022-02-12 11:58:45
tags:
categories: 数据结构和算法
---

# Heap

+ Complete [guide](https://www.programiz.com/dsa/heap-sort) with code

## Concepts

- Physical structure: Array 

<!--more-->

- Logic structure: complete binary tree.

  <img src="https://lh3.googleusercontent.com/pw/AM-JKLWjAFO77pgAh8ppF1JvwBisHva-1g_Q7s-jNLTYPJWVOPFL-NnAfVnNtnX5dBxYJ4aCLGIMcDxQUbIg3yzgZWZNJgjS3D_xiOliuNvJKtk44FjxUoUgK8XwFgG0s4ukcgVs1CpXJKntOZ5SZJ_mn8jc=w322-h248-no?authuser=2" style="zoom:33%;" />

- **只是数组被看作complete binary tree 处理，本质还是对array进行操作**

  - array index `i`, left child `2i+1`, right child `2i+2`
  - array index `i`, parent index $\lfloor \frac{i-1}{2} \rfloor$

- Max and Min (all parents are greater or smaller than their children)
	
	- Max heap, root node largest. 
	- Min heap, root node smallest. 

![](https://lh3.googleusercontent.com/pw/AM-JKLVWfK4STr75ifDy2o3ImjlQKiFUG1QykcXRdpyUGATPL95PfmmdSVnWnwjzEBjybarLUBMW0jsQZKKIUzqj9O-CLATpeEORV2dVD6F8zpVX1CzoAnJ5oOemhFcuvyI53UhQ6fh1rrvePcRA3tPslHJj=w1000-h599-no?authuser=0)

## Build Heap

+ heapify on all non-leaf elements （recursion 自低向上）

  + Heapify: 让parent大于左右孩子

    ```java
    void heapify(int arr[], int n, int i) {
      // Find largest among root, left child and right child
      int largest = i;
      int left = 2 * i + 1;
      int right = 2 * i + 2;
    
      if (left < n && arr[left] > arr[largest]) 
      // compelete binary tree 允许最底层 左为空，所以可能超界
        largest = left;
    
      if (right < n && arr[right] > arr[largest])
        largest = right;
    
        // Swap and continue heapifying if root is not largest
        if (largest != i) {
          swap(&arr[i], &arr[largest]);
          heapify(arr, n, largest);
          // 让原来的root从顶到上找到它的位置，保证左右的sub-tree都是Max-Heap
      }
    }
    ```

  + all non-leaf elements

    ```Java
    for (int i = n / 2 - 1; i >= 0; i--)
          heapify(arr, n, i);
    ```

## Heap Sort （整理）

+  前提：Max - Heap

+ SWAP：

  + 让 root 和最后一个 element 交换
  + reduce heap size by 1 (即把最大元素从Heap中移出来)
  + Heapify the root element
  + Repeat

  ```
      for (int i = n - 1; i >= 0; i--) {
        swap(&arr[0], &arr[i]);
  
        // Heapify root element to get highest element at root again
        heapify(arr, i, 0);
      }
  ```

  

  <img src="D:\file\markdown图片\heap_sort.png" alt="procedures for implementing heap sort" style="zoom:50%;" />

### code

```python
  def heapify(arr, n, i):
      # Find largest among root and children
      largest = i
      l = 2 * i + 1
      r = 2 * i + 2
  
      if l < n and arr[i] < arr[l]:
          largest = l
  
      if r < n and arr[largest] < arr[r]:
          largest = r
  
      # If root is not largest, swap with largest and continue heapifying
      if largest != i:
          arr[i], arr[largest] = arr[largest], arr[i]
          heapify(arr, n, largest)
  
  
  def heapSort(arr):
      n = len(arr)
  
      # Build max heap
      for i in range(n//2, -1, -1):
          heapify(arr, n, i)
  
      for i in range(n-1, 0, -1):
          # Swap
          arr[i], arr[0] = arr[0], arr[i]
  
          # Heapify root element
          heapify(arr, i, 0)
  
  
  arr = [1, 12, 9, 5, 6, 10]
  heapSort(arr)
  n = len(arr)
  print("Sorted array is")
  for i in range(n):
      print("%d " % arr[i], end='')
```

### Heap Sort Complexity

| **Time Complexity**  |           |
| :------------------- | --------- |
| Best                 | O(nlog n) |
| Worst                | O(nlog n) |
| Average              | O(nlog n) |
| **Space Complexity** | O(1)      |
| **Stability**        | No        |

+ `O(n log n)` upper bound on Heapsort's running time and constant `O(1)` upper bound on its auxiliary storage.

## Others

+ Heap Complexity
  + add: O(lgN)
  + remove: O(lgN)
  + search: O(N) (array)
+ application
  + keep extracting the smallest (or largest) element: Priority Queues

## 题目

Leetcode: 347, 23, 295

### LC 347 - Top K Frequent Elements

+  use MinHeap instead of MaxHeap, so that the heap size can be maintained <= k：MinHeap 的 root 就是 第K大
  + 维持一个size k MinHeap
  + 若 `new element > root,` `swap (new ele, root)` (kth will change by heapify)
  + 若 `new element < root`, 保持不变 （root仍是Kth）

### LC 23 - Merge k Sorted Lists

[题解](https://www.bilibili.com/video/BV1X4411u7xF?from=search&seid=11652588004630610482&spm_id_from=333.337.0.0)

+ 用 MinHeap 维持 每个链表头元素 
  + Time: O(nlogk)    Space: O(K)
+ MergeSort

### LC 295 - Find Median from Data Stream

+ one MaxHeap and one MinHeap 
+ key point: keep two heaps balanced

## 其他

- [quicksort random better](https://stackoverflow.com/questions/67390623/why-is-randomised-quicksort-considered-better-than-standard-quicksort)
