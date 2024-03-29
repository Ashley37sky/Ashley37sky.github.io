---
title: C3-Tree
date: 2022-02-12 11:52:39
tags:
categories: [数据结构和算法, 课程]
---

# Tree

## Constructor

### Node Class

<!--more-->

```java
class Node 
{ 
    int key; 
    Node left, right; 
  
    public Node(int item) 
    { 
        key = item; 
        left = right = null; 
    } 
} 
```

---
### Binary Tree Class

```java
class BinaryTree 
{ 
    // Root of Binary Tree 
    Node root; 
  
    BinaryTree() 
    { 
        root = null; 
    } 
  
 }
```

### Build the Tree

```java
BinaryTree tree = new BinaryTree(); 
tree.root = new Node(1); 
tree.root.left = new Node(2); 
tree.root.right = new Node(3); 
tree.root.left.left = new Node(4); 
tree.root.left.right = new Node(5);
```

## Traverse

+ Depth (Breath) First Search

![image-20220225173200561](D:\file\markdown图片\image-20220225173200561.png)

虽然常见的Traverse方法是以上几种, 其实不仅限于上面几种. 以上的DFS都是从左边开始的, DFS还可以从右边开始. 

Code这里有两种方法, 一种是Recursion, 另一种是Iteration, Recursion就不说了, 背的滚瓜烂熟, 说一下Iteration:

- 跟Recursion不同, Iteration的逻辑没有统一性, 有好几种方法, 但无论如何, 没有任何一种方法可以把三者完全统一, 下面用的是postorder和preorder一套逻辑, inorder是另一套自己的逻辑. 
- postOrder的Iteration是最容易理解的, 理解之后看preOrder, 注意要用LinkedList. 
- inOrder是自己一套逻辑, 其实这个逻辑已经跟recursion差不多了, 理解有难度的话, 就背下来吧. 
- **Iteration三方法都是需要熟练掌握背诵的**. 
- Iteration的优势就是在recursion题写不出来的时候, Iteration基于其可控性也许能够帮到你. 

### DFS

#### Recursion 

```java
    /* Given a binary tree, print its nodes in inorder:left-node-right */ 
    void printInorder(Node node) 
    { 
        if (node == null) 
            return; 
  
        /* first recur on left child */
        printInorder(node.left); 
  
        /* then print the data of node */
        System.out.print(node.key + " "); 
  
        /* now recur on right child */
        printInorder(node.right); 
    } 
```

```python
# python - inorder DFS
class Node:
    def __init__(self, data=None, left=None, right=None):
        self.data = data
        self.left = left
        self.right = right
 
def inorder(root):
    
    if root is None:
        return
    
    inorder(root.left)
    print(root.data, end=' ')
    inorder(root.right)
 
 
if __name__ == '__main__':
 
    ''' Construct the following tree
               1
             /   \
            /     \
           2       3
          /      /   \
         /      /     \
        4      5       6
              / \
             /   \
            7     8
    '''
 
    root = Node(1)
    root.left = Node(2)
    root.right = Node(3)
    root.left.left = Node(4)
    root.right.left = Node(5)
    root.right.right = Node(6)
    root.right.left.left = Node(7)
    root.right.left.right = Node(8)
 
    inorder(root)
```

#### Iteration

inorder - (left - node - right)

<img src="D:\file\markdown图片\image-20220225173424943.png" alt="image-20220225173424943" style="zoom:50%;" />

```python
  ## Pseudocode
  s —> empty stack
  while (not s.isEmpty() or node != null)
    if (node != null)  # 向左搜索
      s.push(node)
      node —> node.left
    else 
      node —> s.pop()
      visit(node)
      node —> node.right
```

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> res=new ArrayList<>();
    if (root==null) return res;

    Stack<TreeNode> stack=new Stack<>();
    TreeNode curr=root;

    while(curr!=null || !stack.isEmpty()){
        while (curr!=null){
            stack.push(curr);
            curr=curr.left;
        }
        curr=stack.pop();
        res.add(curr.val);
        curr=curr.right;
    }
    return res;
}
```

```python
from collections import deque
 
class Node:
    def __init__(self, data=None, left=None, right=None):
        self.data = data
        self.left = left
        self.right = right
 
def inorderIterative(root):
    
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

preorder - (node - left - right)

```java
public List<Integer> preorderTraversal(TreeNode root) {
    LinkedList<Integer> ans = new LinkedList<>();
    Stack<TreeNode> stack = new Stack<>();
    if (root == null) return ans;

    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode cur = stack.pop();
        ans.add(cur.val);
        if (cur.right != null) {
            stack.push(cur.right);
        } 
        if (cur.left != null) {
            stack.push(cur.left);
        }
    }
    return ans;
}
```

PostOrder - (left - right - node)  

```java
public List<Integer> postorderTraversal(TreeNode root) {
	// 这里非常重要要用LinkedList, 因为下面要用addFirst. 
	LinkedList<Integer> ans = new LinkedList<>();
	Stack<TreeNode> stack = new Stack<>();
	if (root == null) return ans;
	
	stack.push(root);
	while (!stack.isEmpty()) {
		TreeNode cur = stack.pop();
		ans.addFirst(cur.val);  // 加在开头
		if (cur.left != null) {
			stack.push(cur.left);
		}
		if (cur.right != null) {
			stack.push(cur.right);
		} 
	}
	return ans;
}
```




### BFS


### BFS with level

- BFS 有大概3-4种不同的流派和写法, 目前看了很多还是觉得这个最好. 

```java
class Solution {
    public int maxDepth(TreeNode root) {
        
        if(root==null)
            return 0;
        
        Queue<TreeNode> q = new LinkedList<>();
        // depth用来track 整个tree的深度, 如果不需要这个量, 可不用. 
        int depth = 0;
        q.add(root);
        int n = 1;
        // n用来track 每一层的node数. 
        while(!q.isEmpty())
        {
            depth++;
            for(int i=0 ; i<n ; i++){
                TreeNode thisNode = q.poll();
                if(thisNode.left!=null)
                {
                    q.add(thisNode.left);
                }
                if(thisNode.right!=null)
                {
                    q.add(thisNode.right);
                }
            }
            // n=q.size = 每层的node数
            n = q.size();
        }
        return depth;
    }
}
```


## Types

### Balanced Binary Tree

- a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

![](https://lh3.googleusercontent.com/pw/AM-JKLU0tyNnSzNLcRCQXTE-9FPddX49SrLzoY17Oci5hZGUj2lrzhmAPp6mIXDnxVXnBVSy29t_UkYLPXPE9mDqqxQGO0XXSuhtmYvQBiei8bpSMWe76dPjdX1N5WLPCInD43lHtQQNk0Ytr3CUIeDNhDiI=w342-h221-no?authuser=2)

### Full Binary Tree

- A full binary tree (sometimes proper binary tree or 2-tree) is a tree in which every node other than the leaves has two children.

![](https://lh3.googleusercontent.com/pw/AM-JKLVH3C5BPTQXvefIRKc8jmQrhQ_gfhogQsRev2PQ1vuId77KME6EtZ9MKAAuka57iQjES4ReTSljycc-QZxYdPNcNx0S9lDudC2L7WPIDfUShwG_a8bvkNZScwum6HVzJCQl2hkfJxhqMvFJUAhWVsS_=w320-h300-no?authuser=2)

### Complete Binary Tree

- A complete binary tree is a binary tree in which all the levels are completely filled except possibly the lowest one, which is filled from the left.

![](https://lh3.googleusercontent.com/pw/AM-JKLWjAFO77pgAh8ppF1JvwBisHva-1g_Q7s-jNLTYPJWVOPFL-NnAfVnNtnX5dBxYJ4aCLGIMcDxQUbIg3yzgZWZNJgjS3D_xiOliuNvJKtk44FjxUoUgK8XwFgG0s4ukcgVs1CpXJKntOZ5SZJ_mn8jc=w322-h248-no?authuser=2)

### Binary Search Tree - BST

- A tree is considered a binary search tree (BST) if for each of its nodes the following is true:

	- The left subtree of a node contains only nodes with keys less than the node's key.
	- The right subtree of a node contains only nodes with keys greater than the node's key.
	- Both the left and the right subtrees must also be binary search trees.

![](D:\file\markdown图片\Insert-into-BST.png)

- 技巧, BST的traverse不用上面提到的DFS 和 BFS, 可以直接比大小. 
- 题目: 235 (LCA)

### AVL Tree

不需要掌握. 

- AVL tree is a self-balancing binary search tree. 

![](D:\file\markdown图片\AVL-Tree1.jpg)

### B Tree

不需要掌握. 

-  B-tree is a self-balancing tree data structure that maintains sorted data and allows searches, sequential access, insertions, and deletions in logarithmic time. The B-tree generalizes the binary search tree, allowing for nodes with more than two children.

![](D:\file\markdown图片\Insertion+in+B-Trees+Insertion+in+a+B-tree+of+odd+order.jpg)

### Heap

下节讲

![](D:\file\markdown图片\binaryheap.png)

### TreeMap

以后讲


## 技巧总结

- Tree这一块美国很喜欢考. 因为有点小难度, 又不至于太难为人, 代码量也不大. 
- 这一块是值得刷熟练度的. 如果对递归不是很擅长的话, 建议集中多刷50道Tree.  但没有必要刷难题偏题, 简单中等即可. 不会考太难. 

### 基本模板

- 掌握一套基本的模板. 

### 返回子和返回根

一般的递归分两种情况, 返回子和返回root. 

+ 整个tree做变化的需要返回root. 像下面的变形金刚问题都是需要返回root. 
+ 求某个子的, 比如求lowest common ancestor则需要返回子.  子的返回方式是每一层赋值. 返回root的方式是每一层不赋值, 如下, LC235题答案. 

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if(root == null)
        return null;
    
    if(p.val<root.val && q.val<root.val){
    	// 这里加root = 就可以把底层我们需要的子每一层返回上去, 涂抹掉原本根的值, 最后结果是我们寻找的子. 
    	// 如果不进行涂抹的话, 最后只会返回最高层的root. 
        root = lowestCommonAncestor(root.left, p, q);
    }
    if(p.val>root.val && q.val>root.val){
        root = lowestCommonAncestor(root.right, p, q);
    }
    return root;
    
}
```

### 尝试不同 Traverse

- 一般的题考虑用四种Traverse 方法来解. 如果没有思路的话, 就四种都试一遍. 
  - 先处理根用 preorder
  - 先处理子用 postorder, 
  - BST的话, 从下到上是inorder, 从上到下不用order 直接判断即可.  
  - BFS 很明显, 一般不需要判断. 


## 简单递归

### LC112_hasPathWithGivenSum

> Given a binary tree t and an integer s, determine whether there is a root to leaf path in t such that the sum of vertex values equals s.

```java
boolean hasPathWithGivenSum(Tree<Integer> t, int s) {

    //If just one of left or right was null, then it was not a child node and false can be returned safely
    if(t == null) return false;
    
    //If this is a child AND sum is input, then we have a path
    if(t.left == null && t.right == null) {
        return (s == t.value);
    }
    
    return hasPathWithGivenSum(t.left, s-t.value) || hasPathWithGivenSum(t.right, s-t.value);
}
```



### LC101_isTreeSymmetric

>Given a binary tree t, determine whether it is symmetric around its center, i.e. each side mirrors the other.


```java
//
// Binary trees are already defined with this interface:
// class Tree<T> {
//   Tree(T x) {
//     value = x;
//   }
//   T value;
//   Tree<T> left;
//   Tree<T> right;
// }
boolean isTreeSymmetric(Tree<Integer> t) {
    return isMirror(t,t);
}

boolean isMirror(Tree<Integer> root1, Tree<Integer> root2)
{
    //If both trees are empty, then they are mirror images 
    if (root1 == null && root2 == null)
    {
        return true;
    }
    // For two trees to be mirror images, the following three 
    //     conditions must be true 
    //     1 - Their root node's key must be same 
    //     2 - left subtree of left tree and right subtree 
    //       of the right tree have to be mirror images 
    //     3 - right subtree of left tree and left subtree 
    //        of right tree have to be mirror images 

    if (root1 != null && root2 != null)
    {
        if  (root1.value.equals(root2.value))
        // Java比较能用equals都用equals, 之前跑不过就是因为没用equals. 
        {
            return isMirror(root1.left, root2.right) && isMirror(root1.right, root2.left);
        }   
    }
 

    //If none of these requirement were met, return false. 
    return false;
}
```

### LC230_kthSmallestInBST

- Given a binary search tree t, find the kth smallest element in it.


```java
//
// Binary trees are already defined with this interface:
// class Tree<T> {
//   Tree(T x) {
//     value = x;
//   }
//   T value;
//   Tree<T> left;
//   Tree<T> right;
// }

int answer, k;
// Recursion 之中, 重要的参数尽量放在 recursion之外, 不然recursion之间的参数极易发生混乱. 
int kthSmallestInBST(Tree<Integer> t, int k) {

    this.k = k;
    inorderTraverse(t);

    return answer;
}

void inorderTraverse(Tree<Integer> t)
{
    if(t == null)
    {
        return;
    }

    inorderTraverse(t.left);

    k--;
    if(k == 0)
    {
        answer = t.value;
    }

    inorderTraverse(t.right);
}
```
---
<br><br>

### LC572 isSubTree

```java
//
// Binary trees are already defined with this interface:
// class Tree<T> {
//   Tree(T x) {
//     value = x;
//   }
//   T value;
//   Tree<T> left;
//   Tree<T> right;
// }

//没有简单办法, Traverse 整个 Tree, 然后在check每个Node是否相同. 
boolean isSubtree(Tree<Integer> T, Tree<Integer> S) {

    /* base cases */
    if (S == null)  
        return true; 
   
    if (T == null) 
        return false; 
   
    /* Check the tree with root as current node */
    if (areIdentical(T, S))  
        return true; 
   
    /* If the tree with root as current node doesn't match then 
       try left and right subtrees one by one */
    return isSubtree(T.left, S) || isSubtree(T.right, S); 
}


boolean areIdentical(Tree<Integer> root1, Tree<Integer> root2)  
{ 

    /* base cases */
    if (root1 == null && root2 == null) 
        return true; 

    if (root1 == null || root2 == null) 
        return false; 

    /* Check if the data of both roots is same and data of left and right 
        subtrees are also same */
    return (root1.value.equals(root2.value)
            && areIdentical(root1.left, root2.left) 
            && areIdentical(root1.right, root2.right)); 

} 
```



### LC257_Binary Tree Paths


```java
class Solution {
    List<String> output = new LinkedList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        preOrder(root, "");
        return output;
    }
    
    private void preOrder(TreeNode root, String s){
        if(root == null)
            return;
        
        if(root.left == null && root.right ==null){
            s+=root.val;
            output.add(s);
            //sb.deleteCharAt(sb.length()-1);
            return;
        }
        s+=(root.val + "->");
        preOrder(root.left, s);
        preOrder(root.right,s);
        //sb.deleteCharAt(sb.length()-1);
    }
}
```

### LC235_Lowest Common Ancestor of a Binary Search Tree

- 235和236 都是 FB高频. 

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        // Value of p
        int pVal = p.val;

        // Value of q;
        int qVal = q.val;

        // Start from the root node of the tree
        TreeNode node = root;

        // Traverse the tree
        while (node != null) {

            // Value of ancestor/parent node.
            int parentVal = node.val;

            if (pVal > parentVal && qVal > parentVal) {
                // If both p and q are greater than parent
                node = node.right;
            } else if (pVal < parentVal && qVal < parentVal) {
                // If both p and q are lesser than parent
                node = node.left;
            } else {
                // We have found the split point, i.e. the LCA node.
                return node;
            }
        }
        return null;
    }
}
```

## 中等难度

### 199. Binary Tree Right Side View

- BFS vs DFS **重要, DFS+层数**

### LC1120_Maximum Average Subtree

照标准答案改的, 标准答案太啰嗦了.  **重点在于新建 class 记录**

```java
class Solution {
    // for each node in the tree, we will maintain three values
    class State {
        // count of nodes in the subtree
        int nodeCount;

        // sum of values in the subtree
        int valueSum;

        State(int nodes, int sum) {
            this.nodeCount = nodes;
            this.valueSum = sum;
        }
    }

    private double maxAverage;
    public double maximumAverageSubtree(TreeNode root) {
        maxAverage(root);
        return maxAverage;
    }

    private State maxAverage(TreeNode root) {
        if (root == null) {
            return new State(0, 0);
        }

        // postorder traversal, solve for both child nodes first.
        State left = maxAverage(root.left);
        State right = maxAverage(root.right);

        // now find nodeCount, valueSum and maxAverage for current node `root`
        int nodeCount = left.nodeCount + right.nodeCount + 1;
        int sum = left.valueSum + right.valueSum + root.val;
        maxAverage = Math.max((1.0 * (sum)) / nodeCount, maxAverage); 

        return new State(nodeCount, sum);
    }
}
```

### 236. Lowest Common Ancestor of a Binary Tree

- FB 高频, 插旗子. 

### 297. Serialize and Deserialize Binary Tree

## 变形金刚

变形金刚有以下几个特点: 

- 不会构建新的Tree, 而是在原有tree的基础之上扭来扭去造成变化 
- 难点不是扭, 而是如何处理扭加递归.  正是因为这两样结合, 所以这一系列照比上面单纯的递归要难很多, 通常是medium. 


### LC_226 Invert Binary Tree

原题[在此](https://leetcode.com/problems/invert-binary-tree/)

原题缺乏描述. 其实这个invert是镜像invert, 也就是镜子里面原tree的样子.  这个解法很精简, 用递归, 用的是postorder traverse. 


```java
// 这个其实就是postOrder, 关键在于想明白其实是先把子换了, 最后换parent. 
class Solution {
    public TreeNode invertTree(TreeNode root) {
        
        if( root == null){
            return null;
        }
        // 下面两行对调也没有关系. 无论left 和right 哪个在前都不影响. 
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
}
```

### LC_114 Flatten Binary Tree to Linked List

### LC_426 Convert Binary Search Tree to Sorted Doubly Linked List


### LC_156 Binary Tree Upside Down

跟上面114一样是一道扭来扭去的题目.  整理的答案在下面, 看code是看不懂的, 要看上面的注释,很清晰: 

```java
class Solution {
    /**
        When we turn a simple tree upside down: 
                Root                Left
                /  \                /  \
             Left  Right        Right  Root
        So recursively go to the most left child. Since that will be our new root. And we populate the that child all the way up. So it's visiting root -> root.left -> root.left.left -> root.left.left.left....
        And when its at current root, we make root.left.left = right. root.left.right = root. (We are basically assign left and right child to root.left - new root). Then delete root.left and root.right
           1
          / \   root=2 now            root=1 now
         2   3    ---->      4   1   ------------>   4
        / \                 / \ / \                 / \
       4   5               5   2   3               5   2
            (deleted 2's original 4 and 5)            / \
                                                     3   1(deleted 1's original 2 and 3)
    */
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        if(root == null || (root.left == null && root.right == null))
            return root;
        TreeNode newRoot = upsideDownBinaryTree(root.left);
        root.left.left = root.right;
        root.left.right = root;
        root.left = null;
        root.right = null;
        return newRoot;
    }
}
```

## 面试最重要的是什么? 

- 运气 > 情报 > Behavior > coding
- 情报
	- Facebook  第一轮 phone screen, 只有一个人.  第二轮, 三轮, 一轮 behavior, 两轮 coding
	- 怎么面? 白板面试, 还是写code? 什么语言? 几道题? 
	- 面试范围. 考不考 dp? 
- behavior
- coding
	- 刷公司特定高频高频
	- 解题思路
	- 需要背的知识点
	- 刷 common 高频
	- Object-oriented design

## Time and Space Complexity

- Time complexity: O(n) n as total nodes of the tree.
- Space complexity: O(n)
  - recursion space complexity O(n). Draw back:
  - **Recursion** consumes stack **space**. Every recursive method call produces a new instance of the method, one with a new set of local variables. The total stack space used depends upon the level of nesting of the recursion process, and the number of local variables and parameters.
