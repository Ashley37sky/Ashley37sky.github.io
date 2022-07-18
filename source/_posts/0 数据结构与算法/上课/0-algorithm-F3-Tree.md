---
title: F3_Tree
date: 2022-02-12 11:52:44
tags:
categories: [数据结构和算法, 课程]
---

## F3 - Tree

### LCA 

+ fb 高频

+ 思路：

  <!--more-->

  + 都在左边 -- node.left
  + 都在右边 -- node.right
  + 一左一右 -- node

#### LC 235 - LCA BST

+ Lowest Common Ancestor of a Binary Search Tree

  ```python
  while root:
      if(p.val > root.val and q.val > root.val): # 都在右边
          root = root.right;
      elif(p.val < root.val and q.val < root.val):
          root = root.left;
      else: # 左右各一个 -> root为LCA
          return root;
  ```

#### LC 236 - LCA general

+ Lowest Common Ancestor of a Binary Tree

```python
# L/R没有，返回R/L；LR，返回root
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        if not root:
            return None;
        if root == p or root == q:
            return root;
            
        left = self.lowestCommonAncestor(root.left, p, q);
        right = self.lowestCommonAncestor(root.right, p, q);
        
        if not left: # 左边没有 -> 都在右边
            return right;
        elif not right:
            return left;
        else: # 左右各一个 -> root为LCA
            return root;
```
### 简单

#### LC 112 - path sum

+  whether there is a root to leaf path in t such that the sum of path equals s

```python
class Solution(object):
    def hasPathSum(self, root, targetSum):
        if not root:
            return None
        if(root.left == None and root.right == None):
            return targetSum == root.val
        return self.hasPathSum(root.left, targetSum - root.val) or self.hasPathSum(root.right, targetSum - root.val)
```

#### LC 101 - Symmetric Tree

+ whether it is symmetric around its center, i.e. each side mirrors the other
+ 思路：
  + root1.left == root2.right && root1.right == root2.left

```python
class Solution(object):
    def isSymmetric(self, root):
        return self.ismirror(root, root) # 不写LR，避免None
    
    def ismirror(self, root1, root2):
        if (root1 == None and root2 == None):
            return True;
        if (root1 != None and root2 != None):
            return root1.val == root2.val and self.ismirror(root1.left, root2.right) and self.ismirror(root1.right, root2.left)
        return False
```

#### LC 230 - Kth Smallest in BST

+ Given a binary search tree t, find the kth smallest element in it.
+ 思路：inorder 遍历的第K个  (BST, 左儿子小 右儿子大)
+ 难点：recursion怎么处理global k

```python
class Solution(object): # recurssion
    def kthSmallest(self, root, k):
        self.k = k
        self.ans = 0
        self.inorder(root)
        return self.ans
        
    def inorder(self, node):
        if not node:
            return;
        self.inorder(node.left);
        self.k -= 1
        if not self.k:
            self.ans = node.val
            return
        self.inorder(node.right)
```

```python
class Solution(object): # stack
    def kthSmallest(self, root, k):
        stack = []
        cur = root
        # inorder: left node right
        while stack or cur:
            if cur:
                stack.append(cur)
                cur = cur.left
            else:
                cur = stack.pop()
                k -= 1
                if not k:
                    return cur.val
                cur = cur.right
```

#### LC 572 - Subtree

+ Subtree of Another Tree
+ 递归递归

```python
class Solution(object):
    def isSubtree(self, root, subRoot):

        if subRoot == None:
            return True
        if root == None:
            return False
        
        return self.identical(root, subRoot) or self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)
    
    
    def identical(self, r1, r2):
        
        if not r1 or not r2:
            return r1 == r2 # 同None真，一None假
        
        return r1.val == r2.val and self.identical(r1.left, r2.left) and self.identical(r1.right, r2.right)
```

#### LC 257 - Binary Tree Paths

+ Given the `root` of a binary tree, return *all root-to-leaf paths in **any order***.
+ 关键：怎么存路径

```python
class Solution(object):
    def binaryTreePaths(self, root):
        self.output = []
        self.search(root, '')
        return self.output
        
    def search(self, node, s):
        if not node:
            return
        if not node.left and not node.right: # leaf
            s += str(node.val)
            self.output.append(s)
            return
        
        s += str(node.val) + '->'
        self.search(node.left, s)
        self.search(node.right, s)
```

+ 用 str 不需要 pop，因为 string 是primitive type，不是 object，值不会传入下一层

+ 若是返回 list，那么加入后还需要删除 （是object）

  ```python
  ans.append(node)
  self.search(node.left, ans)
  self.search(node.right, ans)
  ans.pop(node)
  ```

  

### 中等

#### LC 199 - Right Side View

+ Binary Tree Right Side View
+ DFS key：
  + 从右向左
  + depth == len(self.ans) 添加ans
  + `if depth < len(self.ans): return`  错误：如果左边比右边深，还需要向下搜索

```python
class Solution(object):
    def rightSideView(self, root):
        self.ans = []
        self.search(root,0)
        return self.ans
        
    def search(self, node, depth):
        if not node:
            return 
        if depth == len(self.ans): # 当前层还未添加
            self.ans.append(node.val)
        self.search(node.right, depth+1)
        self.search(node.left, depth+1)
```

+ BFS key:
  + deque

```python
class Solution(object):
    def rightSideView(self, root):
        if not root:
            return None
        deque = collections.deque()
        deque.append(root)
        ans = []
        while deque:
            n = len(deque)
            for i in range(n):
                node = deque.popleft()
                if i == n-1: # 最后即是最右
                    ans.append(node.val)
                if node.left:
                    deque.append(node.left)
                if node.right:
                    deque.append(node.right)
        return ans
```

#### ？ LC 1120 - subscribe

#### ？ LC 297 - Serialize and Deserialize Binary Tree 

+ 思路：BFS

### 变形金刚

+ 直接再树上做变换

#### LC 226 - Invert Binary Tree

+ 以根为轴翻转

```python
class Solution(object):
    def invertTree(self, root):
    
        if not root:
            return root
        
        root.left, root.right = root.right, root.left       
        self.invertTree(root.left)
        self.invertTree(root.right)
        
        return root
```

#### LC 114 - Flatten Binary Tree to Linked List

<img src="D:\file\markdown图片\image-20220313214916608.png" alt="image-20220313214916608" style="zoom:50%;" />

+ preorder flatten 

```python
class Solution(object): # 笨办法:空间消耗大
    
    def flatten(self, root):
        if not root:
            return None
        
        self.flatten(root.left)
        self.flatten(root.right)
        
        temp = root.right
        root.right = root.left
        root.left = None
        while root.right:
            root = root.right
        root.right = temp
        
```

+ 思路: （右左中 - 从右到左 post order）
  + 把右子树放到左子树的最右边 （右边的一定比左边的位置后）
  + 把左子树移动到右子树位置 （链表最终都在右儿子上），左儿子=Null
  + update root.right

```python
class Solution(object):
    def flatten(self, root):
        curr = root
        while curr:
            if curr.left: 
                predecessor = nxt = curr.left 
                while predecessor.right:
                    predecessor = predecessor.right
                predecessor.right = curr.right 
                curr.left = None
                curr.right = nxt
            curr = curr.right
```

+ 思路：（preorder遍历，postorder处理）

  ```python
  class Solution(object):
      
      def __init__(self):
          self.pre = None
      
      def flatten(self, root):
  
          if not root:
              return;
          self.flatten(root.right);
          self.flatten(root.left);
          root.right = self.pre; # self.pre子树已经flattern了
          root.left = None;
          self.pre = root;
  ```

#### ？ LC 426 - subscribe

#### ？ LC 156 - subscribe

# 遍历模板

## DFS

### Recurssion

```python
def inorder(root):
    if not root:
        return
    inorder(root.left)
    print(root.data)
    inorder(root.right)
```

### Stack

+ inorder

  ```python
  stack = deque()
  curr = root
   
  # if the current node is None and the stack is also empty, we are done
  while stack or curr:
   
      if curr:
          stack.append(curr)
          curr = curr.left
      else:
          # otherwise, if the current node is None, pop an element from the stack,
          curr = stack.pop()
          print(curr.data, end=' ')
          curr = curr.right
  ```

+ preorder - (node - left - right)

  ```python
  stack = [root]
  while stack():
      cur = stack.pop()
      ans.add(cur.val)
      if cur.right:
          stack.append(cur.right)
      if cur.left:
          stack.append(cur.left)
  ```

+ postorder (left right node)

  ```python
  from collections import deque
  
  ans = deque()
  stack = []
  if root:
      return ans
  stack.append(root)
  while stack:
      cur = stack.pop()
      ans.appendleft(cur.val)
      if cur.left:
          stack.append(cur.left)
      if cur.right:
          stack.append(cur.right)
  print(ans)
  ```

## BFS

```python
def bfs_depth(root):
    if not root:
        return 0
    
    q = [root]
    depth = 0
    while q:
        depth += 1
        temp = []
        for i in range(len(q)):
            if q[i].left:
                temp.append(q[i].left)
            if q[i].right:
                temp.append(q[i].right)
        q = temp[:]
    return depth
```

