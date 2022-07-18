---
title: C5-Matrix Traversal
date: 2022-02-18 15:18:58
tags:
categories: [数据结构和算法, 课程]
---

# Matrix Traversal

题目见`练习`


## 技巧

- DFS 模板
- 染色, 去色（需要回溯）

<!--more-->


## Problems （没做）

- Leetcode练习: 1730, 岛系题目, 490
- 剑指: 12(涉及回溯), 47涉及动态规划. 基本都涉及其他知识. 

## 其他

- Chess

# 练习

## LC 200 - Number of Islands （DFS）

+ [No Iteration](https://leetcode.com/problems/number-of-islands/discuss/1731734/Iterative-Solution-(using-DFS-Java)) and [python Iteration ](https://leetcode.com/problems/number-of-islands/discuss/1758785/Python-DFS-solution) (不推荐，复杂难写)

+ Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

+ key: 代码易读性
  + Check Boundary

```python
class Solution(object):
    def numIslands(self, grid):
        
        def checkBoundary(row, col):
            return row>=0 and col>=0 and row<ROW and col<COL and grid[row][col] == '1'
            
        def dfs(row, col):
            if checkBoundary(row, col):
                grid[row][col] = '0'

                dfs(row + 1, col)
                dfs(row - 1, col)
                dfs(row, col + 1)
                dfs(row, col - 1)
                
              
        ans = 0
        ROW, COL = len(grid), len(grid[0])
        
        for row in range(ROW):
            for col in range(COL):
                if grid[row][col] == '1':
                    dfs(row, col)
                    ans += 1
        
        return ans
```

## LC 79 - Word Search （Backtrack）

+ Given an `m x n` grid of characters `board` and a string `word`, return `true` *if* `word` *exists in the grid*.
+ key: backtrack, 记得还原

```python
class Solution(object):
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        
        if not word:
            return True
        if not board:
            return False
        
        ROW, COL = len(board), len(board[0])
        
        def checkBoundary(row, col ,index):
            if row >= 0 and col >= 0 and row < ROW and col < COL and board[row][col] == word[index]:
                return True
                        
        
        def search(row, col, index):
            if index >= len(word):
                return True
    
            if checkBoundary(row, col ,index):
                char = board[row][col]
                board[row][col] = "#"
                path = search(row+1, col, index + 1) or search(row, col+1, index + 1) or search(row-1, col, index + 1) or search(row, col-1, index + 1)
                board[row][col] = char  # 记得还原 
                return path
            
            return False
        
             
        for row in range(ROW):
            for col in range(COL):
                if search(row, col, 0):
                    return True
        return False
```

## LC 994 - Rotting Oranges (BFS)

+ 多少天橘子全部烂完（烂橘子感染四周的好橘子）

```python
 class Solution(object):
    def orangesRotting(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        visit, curr = set(), deque()
        days = 0
        
		# find all fresh and rotten oranges
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1: # 避免最后还需要遍历一次看有无新鲜
                    visit.add((i, j))
                elif grid[i][j] == 2:
                    curr.append((i, j))
                    
        while visit and curr:
            for _ in range(len(curr)):
                i, j = curr.popleft()  # obtain recent rotten orange
                for coord in ((i-1, j), (i+1, j), (i, j-1), (i, j+1)):
                    if coord in visit:  # check if adjacent orange is fresh
                        visit.remove(coord)
                        curr.append(coord)
            days += 1
            
		
        if visit: # check if fresh oranges remain
            return -1 
        return days
```

## LC 827 - Making A Large Island

+ You are given an `n x n` binary matrix `grid`. You are allowed to change **at most one** `0` to be `1`.
+ key:
  + 怎么区分连接的是一个岛（ans = island[i] + 1）还是两个岛屿 (ans = island[i] + [j]) --> 不同岛染不同颜色
  + 怎么maximum --> 遍历所有零界点，求maximum

```python
class Solution(object):
    def largestIsland(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
  
        n = len(grid)
        
        def CheckBoundary(row, col):
            return row>=0 and col>=0 and row<n and col<n
        
        
        def dfs(row, col, index):
            areas = 0
            if CheckBoundary(row, col) and grid[row][col] == 1:
                grid[row][col] = index
                areas += 1
                areas += dfs(row+1, col, index) + dfs(row-1, col, index) + dfs(row, col+1, index) + dfs(row, col-1, index)
            return areas
        
        def cal_sum(row, col):
            area = 0
            visited = set()
            for i, j in [(-1, 0), (0, -1), (1, 0), (0, 1)]: # 避免相同岛屿重复加
                if CheckBoundary(row+i, col+j) and grid[row + i][col + j] not in visited:
                    area = area + dic[ grid[row + i][col + j] ]
                    visited.add(grid[row + i][col + j])
            return area
                    
        
        index = 2
        dic = {0: 0} # 颜色:面积
        for row in range(n):
            for col in range(n):
                if grid[row][col] == 1:
                    dic[index] = dfs(row, col, index) # 上色
                    index += 1
                    
        ans = max(dic.values())
        for row in range(n):
            for col in range(n):
                if grid[row][col] == 0:
                    temp = cal_sum(row, col) + 1
                    ans = max(ans, temp)
                    
        return ans
```

## 1730

## 490

