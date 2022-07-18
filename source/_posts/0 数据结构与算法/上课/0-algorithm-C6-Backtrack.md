---
title: C5-Backtrack
date: 2022-02-23 15:40:35
tags:
categories: [数据结构和算法, 课程]
---

# Backtrack 回溯法

+ 介绍

  + 回溯法本质就是利用递归进行暴力搜索，解决 for 循环不好解决的问题 

  + 回溯法可以抽象 Tree

    <!--more-->

    + for -  平层遍历

    + 递归 - 向下（深度）走

      ```
                         root
              1        2          3        4
            (234)     (34)       (4)       ()
          2  3   4     3  4        4            
       (34) (4)   ....
      ```

  + 模板

    ```python
    def backtrack(start, path):
        if 终止条件:
            ans.append(path[:]) # 要复制 element，不然指向同一个地方
            return
        
        for i in range(start, n):
            path.append(num[i])
            backtrack(i+1, path)
            path.pop()
    ```

  + 进阶：减枝操作

+ 补充：

  + **在python中是没有自增和自减的，但在python中存在 i = i + 1和 i = i -1 的情况。**

    **因为Python的模型规定，数值对象是不可改变的。 i = i + 1 相当于重新创建了一个变量 i ，而不是改变了 i 中的数值。**

    **++i = +(+i)**   --> ++2 equals 2


```python
    ans = [1]
    temp = [2]
    ans.append(temp) # [1, [2]]
    ans += temp # [1, 2]  必须是两个list的连接
```

## 题目介绍

### 简单

- 77 Combinations
- 78 Subsets
- 46 Permutations

### 核心

Backtrack的核心是三道题. 这三道题覆盖了很多其他题目.

- 40 Combination Sum II  (组合)
- 47 Permutations II  (排列)
- 90 Subsets II  (组合). 


### 好题

好题是难度跟核心类似但普遍性稍差的题目. 

- 131 Palindrome Partition 这道本质不难. 就是复合题.  i+1组合复杂条件判断组合双指针. 
- 698 Partition to K Equal Sum Subsets. 跟上面的131一样, 也是i+1条件判断跳跃. 只不过这道更难更复杂. 目前在条件判断跳跃之中, 这道是最难的.   可以理解为更加复杂的40题. 40题如果刷熟了的话可以刷这道. 

### 题号

* 78 Subsets (combination)
* 90 Subsets II(必刷)
* 46 Permutations
* 47 Permutations II (必刷)
* 77 Combinations
* 39 Combination Sum (简单)
* 40 Combination Sum II (必刷)
* 216 Combination Sum III
* 377 Combination Sum IV (没做)
* 131 Palindrome Partition
* 132 Palindrome Partitioning II (DP)
* 1278 Palindrome Partitioning III (DP)
* 1745 Palindrome Partitioning IV (DP)
* 267 Palindrome Permutation II (和47题重复)
* 17 Letter Combinations of a Phone Number
* 698 Partition to K Equal Sum Subsets

偏题/难题: 

- 60  Permutation Sequence
- 1048 Longest String Chain

应用题: 

- 1152
- 526 Beautiful Arrangement
- 126

# 题目

## 类型一：组合

```python
def backtrack(start, path): # 模板
    if 终止条件:
        ans.append(path[:])
        return
    
    for i in range(start, n): # 不往回走
        path.append(num[i])
        if i != start and nums[i] == nums[i-1]: # 查重
            continue
        backtrack(i+1, path)
        path.pop()
```

#### LC78 - Subsets

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

```python
class Solution: # 推荐写法
    def subsets(self, nums):
        
        def dfs(start, curr):
            # 这里重要, 必须curr[:]的意义跟 java讲过的是一样的, 必须新建一个list
            output.append(curr[:])
            
            for i in range(start, n):
                curr.append(nums[i])
                dfs(i + 1, curr)
                curr.pop()
        
        output = []
        n = len(nums)
        dfs(0, [])
        return output
```

+ 其他解法

  ```python
  class Solution(object): # 解法1 - 递推
      def subsets(self, nums):
  
          ans= [[]]
          
          for num in nums:
              ans += [ cur + [num] for cur in ans ] # num:int [num]:list
              
          return ans
  ```

  ```python
  class Solution(object): # 解法2 - 二进制映射
      def subsets(self, nums):
      
          ans = []
          n = len(nums)
          
          for i in range(2**n, 2**(n + 1)):
              bitmask = bin(i)[3:] # 0bxxxx
              ans.append([nums[j] for j in range(n) if bitmask[j] == '1'])
              
          return ans
  ```

  ```python
  class Solution(object): # backtrack 按照长度输出
      def subsets(self, nums):
      
          n = len(nums)
          ans= []
          
          def backtrack(start = 0, cur = []):
  
              if len(cur) == k:
                  ans.append(cur[:]) 
                  return
                  
              for i in range(start, n):
                  cur.append(nums[i])
                  backtrack(i + 1, cur) # 往前搜索
                  cur.pop() # 回退
          
          for k in range(n+1): # length from 0 to n
              backtrack()
          
          return ans
  ```

#### LC90 - Subsets II

```
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]  # 不会有两个[2]
```

```python
class Solution(object):
    def subsetsWithDup(self, nums):

        def dfs(start = 0, cur = [], summ = 0):

            ans.append(cur[:])
            
            for i in range(start, n):
                if i != start and nums[i] == nums[i-1]: # NO REPEAT
                    continue
                cur.append(nums[i])
                dfs(i+1, cur, summ + nums[i])
                cur.pop()
        
        nums.sort()
        n = len(nums)
        ans = []
        dfs()
        return ans
```

#### LC77_Combinations

+ Given two integers `n` and `k`, return *all possible combinations of* `k` *numbers out of the range* `[1, n]`

  ```
  Input: n = 4, k = 2
  Output:
  [
    [2,4],
    [3,4],
    [2,3],
    [1,2],
    [1,3],
    [1,4],
  ]
  ```

```python
class Solution(object):
    def combine(self, n, k):
        
        def dfs(start = 0, cur = [], l = 0):
            if l == k:
                ans.append(cur[:])
                return
            
            for i in range(start, n):
                cur.append(i + 1)
                dfs(i + 1, cur, l + 1)
                cur.pop()
        
        ans = []
        dfs()
        return ans
```

#### LC39 - Combination Sum

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
```

```python
class Solution(object):
    def combinationSum(self, candidates, target):
        
        def dfs(start = 0, cur = [], summ = 0):
            
            if summ >= target:
                if summ == target:
                    ans.append(cur[:])
                return
            
            for i in range(start, n):
                cur.append(candidates[i])
                dfs(i, cur, summ + candidates[i])  # 避免了在cur.append后和pop和修改sum
                cur.pop()
                
                
        n = len(candidates)
        ans = []
        dfs()
        return ans
```

#### LC40 - Combination Sum II （查重）

```PYTHON
Input: candidates = [2,5,2,1,2], target = 5 # [1,2,2,2,5]
Output:  # 不允许重复数组
[
[1,2,2], # 有3个2，但是[1,2,2]只能出现一此
[5]
]
```

+ Key: continue, return
  + 一开始要去重, 通过去重避免重走相同元素. 去重分两步, 一开始要sort, 之后backtrack的时候要查跟前一个元素是否一

```python
class Solution(object):
    def combinationSum2(self, candidates, target):
        
        def dfs(start = 0, cur = [], summ = 0):

            if summ == target:
                ans.append(cur[:])
                return
            
            for i in range(start, n):
                if i != start and candidates[i] == candidates[i-1]: # 平层无重复，类似信息论 无前缀编码
                    continue
                if summ + candidates[i] > target: # 剪枝：不用向下搜索，加快速度
                    return
                cur.append(candidates[i])
                dfs(i+1, cur, summ + candidates[i])
                cur.pop()
        
        candidates.sort() # 方便去重
        n = len(candidates)
        ans = []
        dfs()
        return ans
```

## 类型二：排列

+ key：
  + 不从 start 开始走，从头开始走。但是需要记录某一个元素时候走过 `flag`
  + 查重 --> 记录count （思想：`无前缀编码`）

#### LC46_Permutations

+ ```
  Input: nums = [1,2,3]
  Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
  ```

```python
class Solution(object):
    def permute(self, nums):
        
        def dfs(cur = [], flag = [], l = 0):
            if l == n:
                ans.append(cur[:])
                return
            
            for i in range(n): # 不从start开始，can go back
                if nums[i] not in flag: # no repeat element
                    cur.append(nums[i])
                    flag.append(nums[i])
                    dfs(cur, flag, l+1)
                    cur.pop()
                    flag.pop()
        
        n = len(nums)
        ans = []
        dfs()
        return ans
```

#### LC47 - Permutations II （查重）

+ Given a collection of numbers, `nums`, that might contain duplicates, return *all possible unique permutations **in any order**.*

  ```PYTHON
  Input: nums = [1,1,2]
  Output:
  [[1,1,2], # 不允许重复 不会出现两个 [1,1,2]
   [1,2,1],
   [2,1,1]]
  ```

+ key: 避免重复，用Hashmap存数量 （重复类比`无前缀编码`）

  [讲解](https://www.youtube.com/watch?v=qhBVWf0YafA&list=PLot-Xpze53lf5C3HSjCnyFghlW0G1HHXo&index=4)

```python
class Solution(object):
    def permuteUnique(self, nums):
        
        def dfs(cur = []):
            
            if len(cur) == n:
                ans.append(cur[:])
                return
            
            for key in count: # 不从start开始，can go back
                if count[key] > 0:
                    cur.append(key)
                    count[key] -= 1
                    dfs(cur)
                    cur.pop()
                    count[key] += 1
                 
        n = len(nums)
        ans = []
        count = {}
        
        for i in range(n):
            count[nums[i]] = count.get(nums[i], 0) + 1
               
        dfs()
        
        return ans
```
