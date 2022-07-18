---
title: F2_指针和滑动窗口
date: 2021-11-29 15:40:30
tags: 
categories: [数据结构和算法, 课程]
---

# F2_指针和滑动窗口

## 双向指针

> 关键：游标卡尺 （找到移动条件），一般只关注端点，不关注区间element
>
> 条件：一般 l < r

### LC167 - two sum

+ 题目：two sum - 给定单调不减数组和c，找到一组（a,b）使得 a+b = c

<!--more-->

+ low, high 指针
  + (low + high) > c --> high --
  + (low + high) > c --> low ++

```python
class Solution(object):
    def twoSum(self, numbers, target):
        l, r = 0, len(numbers)-1
        while l < r:
            sum = numbers[l] + numbers[r]
            if sum == target:
                return [l+1,r+1]
            if sum < target:
                l += 1
            else:
                r -= 1
```

### LC15 - three sum

+ 题目：3 sum - 给定数组，找到所有不重复三元组[a,b,c]，a+b+c = 0
+ sort
+ 遍历 num[i] <= 0
  + 找 a+b = -c (同two sum)
  + 存（a,b,c）
  + (不重复) num[low] == num[low-1]，low ++

```python
class Solution(object):
        
    def threeSum(self, nums):
        ans = []
        nums.sort()
        for i in range(len(nums)-1):
            if (nums[i] > 0) or (i>0  and nums[i] == nums[i-1]): # 去重 + 简化
                continue
            l, r = i+1, len(nums)-1
            while l < r:
                s = nums[i] + nums[l] + nums[r]
                if s < 0:
                    l += 1 
                elif s > 0:
                    r -= 1
                else:
                    ans.append((nums[i], nums[l], nums[r]))
                    while l < r and nums[l] == nums[l+1]:  # 去重, 出现nums[i+1] 一定要考虑边界
                        l += 1
                    while l < r and nums[r] == nums[r-1]:
                        r -= 1
                    l += 1; r -= 1
        return ans
```

### LC11 - with most water

+ 题目：with most water - min(height[left], height[right])*(right-left))
+ 贪心思想
+ 左右指针 (保留高的墙壁)
  + ans = max(max, Math.min(height[left], height[right])*(right-left))
  + height[left]>height[right] --> right--
  + height[left]<height[right] --> left ++ 

```python
class Solution(object):
    def maxArea(self, height):
        ans = 0
        l, r = 0 , len(height)-1
        while l < r:
            ans = max(ans, min(height[r],height[l])*(r-l))
            if height[l]>height[r]:
                r -= 1
            else:
                l +=1
        return ans
```

### LC581 - 最短未排序区间

+ 题目: 给定数组，找到最短[a,b], 使得[a,b] 排序后，整个数组都是有序的 O(n)
+ 易错：不能只找拐点
+ 思路1：
  + 遍历两边：从前（后）到后（前），找到拐点a（b）
  + 遍历: [a,b] 找打 max，min
  + 遍历: [0,a] 找min ；[b,end] 找 max

<img src="https://img-blog.csdnimg.cn/20191122233425943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9taWNoYWVsLmJsb2cuY3Nkbi5uZXQ=,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:50%;" />
+ 思路2：排序，再对比第一个不相等元素 （nlogn）

```python
# 思路1
class Solution(object):
    def findUnsortedSubarray(self, nums):
        l,r = 0,len(nums)-1
        while l<len(nums)-1 and nums[l] <= nums[l+1]:
                l += 1
        if l == len(nums)-1:
            return 0
        while r>0 and nums[r-1]<=nums[r]:
            r -= 1
        n_min,n_max = nums[l],nums[l]
        for i in range(l,r+1):
            n_min = min(n_min,nums[i])
            n_max = max(n_max,nums[i])
        l_s, r_s = l, r
        for i in range(l):
            if nums[i] > n_min:
                l_s = i
                break
        for i in range(len(nums)-1,r,-1): # 倒着循环
            if nums[i] < n_max:
                r_s = i
                break
        return r_s+1-l_s
```

```python
class Solution(object):
    def findUnsortedSubarray(self, nums):
    
        a = nums[:] # 复制元素，不然指向同一地址！！
        a.sort()
        n = len(nums)
        
        for i in range(n):
            if nums[i] != a[i]:
                break
        if i == n-1:
            return 0
        for j in range(n-1, 0, -1):
            if nums[j] != a[j]:
                break
        return j-i+1
```

## 同向指针

> like sliding window, 关注区间的端点和中间的element
>
> HashMap: 只关心count or pos, 与顺序无关

### LC438 - find anagrams

+ 题目：Find All Anagrams in a String
+ anagrams -- 存出现频率即可
+ 比较Hashmap (python: dict) / set
  + key: value ==0 和 不存在key, 再Hashmap中是不同的
    + 解决方法1：最开始就初始化全部为“0”
    + 解决方法2：若key对应value为0，删除key: value

```python
class Solution(object):
    def findAnagrams(self, s, p):
        if len(s) < len(p):
            return []
        s_count, p_count = [0] * 26, [0] * 26
        for ch in p:
            p_count[ord(ch)%26] += 1 # 使用set，instead of dict/Hashmap
        ans = []
        
        for i in range(len(s)): # 区间长度给定，单指针就行
            if i < len(p):
                s_count[ord(s[i])%26] += 1
                continue
            if s_count == p_count:
                ans.append(i - len(p))
            s_count[ord(s[i])%26] += 1
            s_count[ord(s[i-len(p)])%26] -= 1
        if s_count == p_count:
            ans.append(len(s)-len(p))
        return ans
```

### LC03 - 最短不重复区间

+ 题目：find the length of the longest substring without repeating characters

```python
# 双指针 若x在set中，则left左移至原 position x +1
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        ans, l ,r = 0, 0 ,0
        string_set = set()
        while r < len(s):
            while s[r] in string_set:
                string_set.remove(s[l])
                l += 1
            string_set.add(s[r])
            r += 1
            ans = max(ans, r-l)
        return ans
```

```PYTHON
# Hashmap  key(字母):value(上一次出现位置)
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        ans, l, r = 0, 0, 0
        s_dict = dict()
        while r < len(s):
            if s_dict.get(s[r]) != None: # 注意 None 和 0
                l = max(l, s_dict[s[r]]+1) # 有重复的最远位置
            s_dict[s[r]] = r
            r += 1 # 左开右闭
            ans = max(ans, r-l)
        return ans
    
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        hashmap = {}
        ans, last = 0, -1
        for i in range(len(s)):
            last = max(hashmap.get(ord(s[i]), -1), last)
            ans = max(ans, i - last)
            hashmap[ord(s[i])] = i
        return ans
```

## 快慢针

> 链表不能定位，用快慢针保持结尾和所求位置的关系

### L876 链表中点

+ Middle of the Linked List

  ```python
  class Solution(object):
      def middleNode(self, head):
          
          slow, fast = head, head
          while fast and fast.next: # 注意条件
              fast = fast.next.next
              slow = slow.next
          return slow
  ```

### LC19 删除链表倒数Kth

+ Remove Nth Node From End of List

  + **dummy head**：避免删除第一个元素

  ```python
  # class ListNode(object):
  #     def __init__(self, val=0, next=None):
  
  class Solution(object):
      def removeNthFromEnd(self, head, n):
          dummy = ListNode(0, head) # 防止第一个元素被移除
          slow, fast = dummy, dummy
          for i in range(n+1):
              fast = fast.next
          while fast:
              fast = fast.next
              slow = slow.next
          slow.next = slow.next.next
          return dummy.next
  ```

## 滑动窗口 / deque

### LC239 Sliding Window Maximum (deque)

+ 给定nums以及窗口大小K，返回每一个滑动窗口中的最大值[]

+ Deque

  + queue 后面进，前面出；stack 后面进，后面出

  + deque 前后都可以进出

  + ![image-20220225154819522](D:\file\markdown图片\image-20220225154819522.png)

+ 算法：

  + 记录max下标，存入deque
  + 窗口移动时
    + 判断是否出界，若出界则popleft
    + num[i] > deque[num[-1]]: deque.pop() --> 后面比前面大，那么前面的不会再用到了
  + 重点：
    + deque[num[0]] 一定是窗口中的max
    + dq 中的element一定是降序排列
  
  ```python
  from collections import deque
  
  class Solution(object):
      
      def maxSlidingWindow(self, nums, k):
          dq = deque() # 存的index
          ans = []
          
          for i in range(len(nums)):
              if dq and i - dq[0] == k: # 区间长度大于k：i -dq[0] + 1 > k 同i-dq[0] == k
                  dq.popleft()
              while dq and nums[dq[-1]] <= nums[i]: # h
                  dq.pop()
              dq.append(i)
              if i >= k-1:
                  ans.append(nums[dq[0]])
              
          return ans
  ```

### LC 992 Subarrays with K Different Integers

+ Given an integer array `nums` and an integer `k`, return *the number of **good subarrays** of* `nums`.

  A **good array** is an array where the number of different integers in that array is exactly `k`.

+ 算法：

  + 滑动窗口找 atmost K --> exactlk k = atmost(nums, k) - atmost(nums, k-1)
  + 对于新元素nums[i]
    + 前面已经出现过，则可以再组成 i - left + 1 个子序列
    + 未出现，则加入；若size超过K，则 left ++

```python
# 写的太丑了 不想改了
def atmost(nums, k):
    if k == 0:  # ans default = 1 
        return 0
    left, ans = 0, 1
    count = dict()
    count[nums[0]] = 1
    for i in range(1, len(nums)):
        if nums[i] in count:
            count[nums[i]] += 1
        else:
            count[nums[i]] = 1
        while (left < len(nums) and len(count) > k):
            if count[nums[left]] == 1:
                count.pop(nums[left])
            else:
                count[nums[left]] -= 1
            left += 1
        ans += i - left + 1  
    return ans

class Solution(object):
    def subarraysWithKDistinct(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        return atmost(nums, k) - atmost(nums, k-1)
```



## Homework

### LC26 删除重复元素

+ [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array)

  + easy: 双指针

### LC42 trapping water rain

+ [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water)

+ 可以存水的条件：height[i] < min(leftmax,rightmax)

+ 法一：O(n) O(n)

  + 遍历找leftmax rightmax
  + 遍历算ans

+ 法二：O(n) O(1)

  + 左右指针
  + 存水取决于min(leftmax,rightmax)
    + leftmax < rightmax --> left ++

  ```python
  # 方法一
  class Solution(object):
      def trap(self, height):
          ans = 0
          n = len(height)
          leftmax, rightmax = [0] * n, [0] * n
          for i in range(1, n):
              leftmax[i] = max(height[i-1], leftmax[i-1])
              rightmax[n-1-i] = max(height[n-i], rightmax[n-i])
          for i in range(n):
              ans += max(min(leftmax[i], rightmax[i]) - height[i], 0)
          return ans
  ```

  ```python
  # 方法二
  class Solution(object):
      def trap(self, height):
          ans = 0
          l, r = 0, len(height)-1
          leftmax, rightmax = 0, 0
          while l <= r: # 注意边界条件
              if leftmax < rightmax:
                  ans += max(leftmax - height[l], 0)
                  leftmax = max(leftmax, height[l])
                  l += 1
              else:
                  ans += max(rightmax - height[r], 0)
                  rightmax = max(rightmax, height[r])
                  r -= 1
          return ans
  ```

### LC763 Partition Labels

  + partition the string into as many parts as possible so that each letter appears in at most one part.
  
  + 第一次做错误原因：直接achor  = pos[s[archor]]+1, 只关注了开头，忽略了子段中间的信息（中间存在字符的最后pos > 头的最后pos）
  
  + 算法
  
    ```python
    class Solution(object):
        def partitionLabels(self, s):
            ans = list()
            pos = {ch: last_pos  for last_pos,ch in enumerate(s)} # emunerate 返回 index, element
            anchor, far = 0, 0
            for index, ch in enumerate(s):
                far = max(far, pos[ch])
                if index == far:  # 注意条件
                    ans.append(index - anchor + 1)
                    anchor = index + 1
            return ans
                
    ```

