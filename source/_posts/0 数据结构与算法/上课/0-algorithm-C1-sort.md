---
title: C1_Sort
date: 2021-11-28 21:56:53
tags: sort
categories: [数据结构和算法, 课程]
---

## 算法内容

- 高频：宽度优先搜索（BFS），深度优先搜索（DFS），二分法（Binary Search），双指针（2 Pointer），堆、栈、队列、哈希表（Heap，Stack，Heap，HashMap/HashSet），前缀和（Prefix Sum），链表（LinkedList），二叉树（Binary Tree），二叉搜索树（Binary Search Tree），快速排序与归并排序（Quick Sort/ Merge Sort）

<!--more-->

- 中频：动态规划（DP），扫描线（Sweep Line），字典树（Trie），并查集（Union Find），单调栈与单调队列（Monotone Stack/ Queue），TreeMap等
- 低频：Dijkstra，树状数组，线段树，最小生成树等

## Bubble Sort

最基本的brutal force sorting.   就是从头到尾一遍一遍的过n次, 如果前一个比后一个大就swap,直到没有swap为止. 

### Algorithm

1. Repeatedly swapping the adjacent elements if they are in wrong order.
2. First go from 0 to n-1, keep swaping so we have the largest element in the end. 
3. Then go from 0 to n-2, and so on. 
4. If no swap happens, then break loop. It will take at most n-1 times to traverse the array. 

![](https://lh3.googleusercontent.com/pw/AM-JKLW4sN6VLP6SZBnhwEHNDHmF94FoT7vPHD4_cY7bNUR5OQ1Qus-cOvonFuololtvlz8rE_WDwJFsn8yZXVxTgFdLFjSD2K-GjF9i7BkI0Pw6dSJFbjczYAcwvon1omvTdHvVoR2qqmVg9P0hcgOD38e0=w901-h535-no?authuser=0)

### Code

```java
// An optimized version of Bubble Sort 
static void bubbleSort(int arr[], int n) 
{ 
    int i, j, temp; 
    boolean swapped; 
    for (i = 0; i < n - 1; i++)  
    { 
    	// 此处用n-1次for loop可以保证最后一定sorting, 也可以用while(!swap), 效果是一样的
        swapped = false; 
        for (j = 0; j < n - i - 1; j++)  
        { 
            if (arr[j] > arr[j + 1])  
            { 
                // swap arr[j] and arr[j+1] 
                temp = arr[j]; 
                arr[j] = arr[j + 1]; 
                arr[j + 1] = temp; 
                swapped = true; 
            } 
        } 
  
        // IF no two elements were  
        // swapped by inner loop, then break 
        // 这是一个optimal的算法, 也可以不用swapped这个量, 直接走n次, 能保证最后一定是sorted. 
        if (swapped == false) 
            break; 
    } 
} 
```
### Space and Time Complexity

- Worst and Average Case Time Complexity: O(n^2). Worst case occurs when array is reverse sorted.

- Best Case Time Complexity: O(n). Best case occurs when array is already sorted.


## Selection Sort

Repeatedly finding the minimum element from unsorted part and putting it at the beginning.

应用: 

- the kth largest element. 
- Top k elements, 
- **any order**

![](https://miro.medium.com/max/700/0*c6jNITnQ_jIvCk-3.png)

```java
void selectionSort(int arr[], int n) 
{ 
    int i, j, min_idx; 
  
    // One by one move boundary of unsorted subarray 
    for (i = 0; i < n-1; i++) 
    { 
        // Find the minimum element in unsorted array 
        min_idx = i; 
        for (j = i+1; j < n; j++) {
          if (arr[j] < arr[min_idx]) 
            min_idx = j; 
        }
  
        // Swap the found minimum element with the first 
        // element 
        int temp = arr[min_idx]; 
        arr[min_idx] = arr[i]; 
        arr[i] = temp;
    } 
} 
```

### Space and Time Complexity

- Worst and Average Case Time Complexity: O(n^2). Worst case occurs when array is reverse sorted.

- Best Case Time Complexity: O(n). Best case occurs when array is already sorted.

## Insertion Sort

也很brutal, 整个array从头到尾刷一遍, 如果后一个比前一个小, 就用while loop跟上一个比较往前挪, 直到挪到适当的位置为止. 

### Algorithm

To sort an array of size n in ascending order:  

1. Iterate from arr[1] to arr[n] over the array.
2. Compare the current element (key) to its predecessor.
3. If the key element is smaller than its predecessor, compare it to the elements before. Move the greater elements one position up to make space for the swapped element.

![](https://media.geeksforgeeks.org/wp-content/uploads/insertionsort.png)

```java
void sort(int arr[]) 
{ 
    int n = arr.length; 
    for (int i = 1; i < n; ++i) { 
        int key = arr[i]; 
        int j = i - 1; 
  
        /* Move elements of arr[0..i-1], that are 
           greater than key, to one position ahead 
           of their current position */
        while (j >= 0 && arr[j] > key) { 
            arr[j + 1] = arr[j]; 
            j = j - 1; 
        } 
        arr[j + 1] = key; 
    } 
} 
```

### Space and Time Complexity

- Worst and Average Case Time Complexity: O(n*n). Worst case occurs when array is reverse sorted.

- Best Case Time Complexity: O(n^2^). Best case occurs when array is already sorted.


## Merge Sort

- 和QuickSort 同为最常见最好用的sorting. 对unsorted list通杀. 优点就是快. 对LinkedList也能用, LC148.
- Merge Sort is a Divide and Conquer algorithm.
- 简单来说就是用二分法把这个array不断二分, 最后分成一个数的时候再互相比较重新拼回去. 
- **Merge sort 是递归. 值得刷!**
- [Best Merge Sort guid I found on Internet](https://www.studytonight.com/data-structures/merge-sort#:~:text=Time%20complexity%20of%20Merge%20Sort,space%20as%20the%20unsorted%20array.)

### Algorithm

- break the given array in the middle. 
- Keep breaking each part until we get each subarray with single element. 
- Since each array only have one element, so they are all sorted. Then we merge these arrays back together. It only takes O(n) to merge two sorted arrays. So the divide will take O(logn), which gives us total time complexity of O(nlogn). 


MergeSort(arr[], l,  r)  
If r > l

 1. Find the middle point to divide the array into two halves:  
         middle m = (l+r)/2
 2. Call mergeSort for first half:   
         Call mergeSort(arr, l, m)
 3. Call mergeSort for second half:
         Call mergeSort(arr, m+1, r)
 4. Merge the two halves sorted in step 2 and 3:
         Call merge(arr, l, m, r)

![](https://www.geeksforgeeks.org/wp-content/uploads/Merge-Sort-Tutorial.png)

```java
import java.util.Arrays;

class MergeSortPractice{
    public static void main(String[] args){
        int[] list = {3,4,2,1,5,0,3,100,2,3,93,24,43};
        mergeSort(list);
        System.out.println(Arrays.toString(list));
    }
}

public static void mergeSort(int[] arr){
    int[] temp =new int[arr.length];
    internalMergeSort(arr, temp, 0, arr.length-1);
}
private static void internalMergeSort(int[] arr, int[] temp, int left, int right){ 
    //当left==right的时，已经不需要再划分了
    if (left<right){
        int middle = (left+right)/2;
        internalMergeSort(arr, temp, left, middle);          //divide left
        internalMergeSort(arr, temp, middle+1, right);       //divide right
        mergeSortedArray(arr, temp, left, middle, right);    //merge
    }
}
// 合并两个有序子序列
private static void mergeSortedArray(int arr[], int temp[], int left, int middle, int right){ 
    int i=left;      
    int j=middle+1;
    int k=0;
    // Sort and combine into a new temp array. O(n)
    while (i<=middle && j<=right){
        temp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
    }
    while (i <=middle){
        temp[k++] = arr[i++];
    }
    while ( j<=right){
        temp[k++] = arr[j++];
    }
    //把数据复制回原数组
    for (i=0; i<k; ++i){
        arr[left+i] = temp[i];
    }
}
```

### Space and Time Complexity

- **没有worst case和best case，时间复杂度稳定是O(nlgn)，但是需要开空间**
- Time Complexity: `O(n*log n)`
- Space Complexity: `O(n)`

## Quick Sort

- 跟 merge sort 一样是divide and conquer
- 相较于merge sort直接二分, quick是选一个节点进行比较, 然后把节点放在对的位置, 保证节点前比其小, 后面比节点都大, 之后再进行二分. 
- 选节点pivot的方法可以自由发挥, 没有固定规定.
- 与merge sort的不同在于拼回去的过程之中无需再比较

### Algorithm

- Select a 'pivot' element from the array and partitioning the other elements into two sub-arrays, one subarray with elements less than the pivot element, the other with elemnts greater or equal to the pivot element. 
- The sub-arrays are then sorted recursively until only one element is left in each subarrays. 

这个图中里选用的是最后一个数作为节点. 

![Quick Sort Illustration](https://www.techiedelight.com/wp-content/uploads/Quicksort.png)

### Code

```java
public static void quickSort(int[] arr){
    qsort(arr, 0, arr.length-1);
}
private static void qsort(int[] arr, int left, int right){
    if(left<right){
        int pivot = partition(list, left, right);
        qSort(list, left, pivot-1);
        qSort(list, pivot+1, right);
    }
}
public static int partition(int[] arr, int left, int right) {
	
    int pivot = arr[left]; // 此处选取Left, 也就是第一个数为Pivot. 
    // 1. move pivot to end
    swap(arr, left, right);
    int store_index = left;

    // 2. move all smaller elements to the left
    for (int i = left; i <= right; i++) {
      if (arr[i] < pivot) {
        swap(arr, store_index, i);
        store_index++;
      }
    }

    // 3. move pivot to its final place
    swap(arr, store_index, right);
    return store_index;
  }

  public static void swap(int[]arr, int a, int b) {
    int tmp = arr[a];
    arr[a] = arr[b];
    arr[b] = tmp;
  }
```


### Space and Time Complexity

For an array, in which partitioning leads to unbalanced subarrays, to an extent where on the left side there are no elements, with all the elements greater than the pivot, hence on the right side.

And if keep on getting unbalanced subarrays, then **the running time is the worst case, which is `O(n^2)`**

Where as if partitioning leads to almost equal subarrays, then the running time is the best, with time complexity as `O(n*log n)`.

- Worst Case Time Complexity [ Big-O ]: `O(n^2)`
- Space Complexity: `O(1)`

## Compare Quick Sort and Merge Sort

- Merge: stable O(nlogn). require O(n) space. 
- Quick: not stable. averge O(nlogn), worst scenario O(n^2). O(1) space. 

## Quick Select

**重点: FB 和 Amazon 最近两年非常喜欢考 LC347 LC973**

![](https://lh3.googleusercontent.com/pw/AM-JKLWNIJ1Gdr26zSrNm4J3_bG1sqNZukMc4vlIDv-_s-pcZ_fZSz35wHKS6FzwElatBCyHOKjWPUZTwgVRIxBIC5djdTmCMWKgHHfPxlogdRJ9aNDPTNpYh5JMTheshW9pbPbr-_-JNfdQPq0K8mtPXuRy=w1015-h783-no?authuser=0)


### LC215

另外一种办法, priority Queue, LinkedList

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        
        this.quickSelect(nums, 0, nums.length - 1, k - 1);
        return nums[k - 1];
    }
    
    private void quickSelect(int[] nums, int left, int right, int k){
        if(left < right){
            int pivot = sort(nums, left, right);
            if(pivot == k){
                return;
            }else if(pivot < k){
                quickSelect(nums, left + 1, right, k);
            }else{
                quickSelect(nums, left, right - 1, k);
            }
        }
    }
    
    private int sort(int[] nums, int left, int right){
        int index = left;
        int pivotValue = nums[index];
        this.swap(nums, index, right);
        
        for(int i = left; i <= right; i++){
            if(nums[i] > pivotValue){
                this.swap(nums, index ++, i);
            }
        }
        
        this.swap(nums, index, right);
        return index;
    }
    
    private void swap(int[] arr, int left, int right){
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
    }
}
```

## Count Sort

O(n)

## Bucket Sort



## Big-O

- [Guide](https://developerinsider.co/big-o-notation-explained-with-examples/)