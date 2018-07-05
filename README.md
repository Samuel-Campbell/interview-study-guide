# Study Guide
This guide provides various problem solving methods depending on the context. All methodologies are sorted in ascending order of implementation difficulty for every category.  
*Please note that this list is subjective to my understanding of the subject. Furthermore the classification of problems and steps followed to resolve them are simply my train of thought written down **and not** an empirical approach to problem solving.*

---

## 1. Arrays
### 1.1 Sorted  
Almost every sorted array problems can be solved using binary search.  

**Binary Search**
```python
def binary_search(array, target):
    mid = len(array) // 2
 
    while True:
        # out of bounds
        if mid < 0 or mid >= len(array):
            return False
        
        # value found
        if target == array[mid]:
            return True
            
        else:
            # cut remainder of array in half
            half = (len(array) - mid) // 2    
 
            # if there is nothing left to cut in half
            if half == 0:
                return False        
            # target greater than value at array index
            elif target > array[mid]:
                mid = mid + half
            # target lower than value at array index
            elif target < array[mid]:
                mid = mid - half
```

### 1.2 Structured  
A structured array implies, that even though the values aren't sorted, there exists some logical sequence to the elements.  

**Does Binary Search work?**  

**Is it a Graph problem?**  
* Hill Climbing --> Finding local maxima
* Traversing the array from point A to point B given special requirements --> DFS/BFS  
  
**Is it a dynamic programming problem?**  
Find a recurrence relationship between the numbers which come before and/or after a value at index i. 

### 1.3 Unstructured and Unsorted  
If section 1.1 and 1.2 are not true then the array is randomly sorted and there are no logical sequences to the value distribution.  

**Linear Search**  
Some problems involve finding particular kinds of values such as the 2 greatest integers.

**Linear Search + Hash (memoize)**  
Some problems involve finding matching pairs of values. In order to avoid backtracking previously seen elements, use a hash to store content as the traversal is performed. **Always** ask if space complexity is an issue before proceeding with this method. If so then skip to quicksort.   

**Merge Sort**  
If sorting is a must then merge sort can be completed in O(nlogn) + O(n)
```python
def merge_sort(array):
    # Base case when array cannot be decomposed further
    if len(array) == 1:
        return array
 
    # index of mid point of the array --> used for splitting
    mid = len(array) // 2
 
    # sort left part
    left_array = merge_sort(array[:mid])
    
    # sort right part
    right_array = merge_sort(array[mid:])
 
    # return sorted array
    return sort(left_array, right_array)
 
 
def sort(sorted_1, sorted_2):
    res = list()
    i = 0
    j = 0
         
    while len(res) != (len(sorted_1) + len(sorted_2)):
        # while index is not out of bounds 
        while i < len(sorted_1):
            # if value 'i' at array 1 is lower than value 'j' at array 2
            if sorted_1[i] < sorted_2[j]:
                res.append(sorted_1[i])
                i += 1
            else:
                break
 
        # if array 1 has been exhausted then simply append all of array 2 and exit
        if i >= len(sorted_1):
            res.extend(sorted_2[j:])
            break
         
        # while index is not out of bounds
        while j < len(sorted_2):
            # if value 'j' at array 1 is lower than value 'i' at array 2
            if sorted_2[j] < sorted_1[i]:
                res.append(sorted_2[j])
                j += 1
            else:
                break
         
        # if array 2 has been exhausted then simply append all of array 1 and exit
        if j >= len(sorted_2):
            res.extend(sorted_1[i:])
            break
    return res
```
**Quick Sort**  
If memory complexity is a problem or the interviewer asks for more than 1 sorting algorithm then quicksort can be performed with an average case of O(nlogn) + O(1).
```python
def quick_sort(array, start, end):
    # Base case when left pointer is greater or equal to right pointer then there is nothing left to sort
    if start >= end:
        return
     
    # pivot using leftmost element and obtain right pointer
    right_pointer = partition(array, start, end)
     
    # sort left half
    quick_sort(array, start, right_pointer - 1)
     
    # sort right half
    quick_sort(array, right_pointer + 1, end)
 
 
def partition(array, pivot, right_pointer):
    # left pointer is always +1 of the pivot
    left_pointer = pivot + 1
    # store leftmost index
    start = pivot
    # store rightmost index
    end = right_pointer
     
    while True:
        # while right value is greater than the pivot keep iterating leftwards
        while (array[right_pointer] > array[pivot]) and (right_pointer > start):
            right_pointer -= 1
         
        # while left value is lower than the pivot keep iterating rightwards
        while (array[left_pointer] < array[pivot]) and (left_pointer < end):
            left_pointer += 1
         
        # if the pointers cross each other then all sorting has been performed
        if left_pointer >= right_pointer:
            break
             
        swap(array, left_pointer, right_pointer)
         
        # swap the pivot value with the right pointer
    swap(array, pivot, right_pointer)
    return right_pointer
 
 
def swap(array, i, j):
    temp = array[i]
    array[i] = array[j]
    array[j] = temp

```

---

## 2. Tree Traversal  
1. Iterative Approach: Create Stack of nodes which have not visited right child
2. Recursive Approach: Base Case --> Left and Right child are Null

```python
class TreeNode(object):
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
```

### 2.1 Inorder
* Visit left --> root --> right  

**Recursive**

```python
def inorder_traversal(root):
    if root is None:
        return
    inorder_traversal(root.left)
    print(root.val)
    inorder_traversal(root.right)
```
**Iterative**
```python
def inorder_traversal(root):
    stack = list()
    while True:
        if root:
            stack.append(root)
            root = root.left
        elif len(stack) > 0:
            root = stack.pop()
            print(root.val) # Do Something
            root = root.right
        else:
            break
```
### 2.2 PreOrder
* Visit root --> left --> right  

**Recursive**
```python
def preorder_traversal(root):
    if root is None:
        return    
    print(root.val) # Do something with node
    pre_order_traversal(root.left)
    pre_order_traversal(root.right)
```

**Iterative**
```python
def preorder_traversal(root):
    stack = list()
    while (len(stack) > 0) or (root is not None):
        print(root.val) # Do something with node
        if root.left:
            stack.append(root)
            root = root.left
        else:
            root = stack.pop()
            root = root.right
```
### 2.3 PostOrder
* Visit left --> right --> root  

**Recursive**
```python
def postorder_traversal(root):
    if root is None:
        return
    postorder_traversal(root.left)
    postorder_traversal(root.right)
    print(root.val) # Do something
```

**Iterative**
```python
def postorder_traversal(root):
    stack = list()
    stack.append(root)
    prev = None
    while len(stack) > 0:
        node = stack[-1]
        if (prev is None) or (prev.left == node) or (prev.right == node):
            if node.left:
                stack.append(node.left)
            elif node.right:
                stack.append(node.right)
        elif node.left == prev:
            if node.right:
                stack.append(node.right)
        else:
            print(node.val) # Do Something
            stack.pop()
        prev = node
```

--- 

## 3 Permutation, SubSet, Combination, Partition  
Certain problem involve finding combinations, sets, permutations, and/or partitions. In these 4 cases then backtracking is easy to use and can solve all these issues.

**Combinations**  
This solution is simply a n choose k implementation.  
Parameters:
* resulting list --> list of combinations
* n --> set of all elements
* k --> number of elements per combination
* combinations --> temporary list
* start --> so that we don't duplicate combinations since order does not matter
```python
def combinations(n, k):
    res = list()
    backtrack(res, n, k)
    return res
 
 
def backtrack(res, n, k, combination=list(), start=0):
    # if enough elements in combination list then append its copy to the results
    if len(combination) == k:
        res.append(combination.copy())
        
    else:
        for i in range(start, len(n)):
            # add element to combination
            combination.append(n[i])
            
            # recursive call
            backtrack(res, n, k, combination, i + 1)
            
            # pop last element
            combination.pop()
```
**Permutations**  
Parameters:
* resulting list --> list of permutation
* nums --> set of all elements
* permutation --> permutation list (temporary)
* index_used --> All values are unique so we memoize the ones that were used
```python
def permutations(nums):
    res = list()
    backtrack(res, nums)
    return res
 
 
def backtrack(res, nums, permutation=list(), index_used=set()):
    if len(nums) == len(permutation):
        res.append(permutation.copy())
    else:
        for i in range(len(nums)):
            if i in index_used:
                continue
            permutation.append(nums[i])
            index_used.add(i)
            backtrack(res, nums, permutation)
            permutation.pop()
            index_used.remove(i)
```
**Sets**  
Parameters:
* resulting list
* nums
* set_list
* start    
```python
def sets(nums):
    res = list()
    backtrack(res, nums)
    return res
 
 
def backtrack(res, nums, set_list=list(), start=0):
    res.append(set_list.copy())
    for i in range(start, len(nums)):
        set_list.append(nums[i])
        backtrack(res, nums, set_list, i + 1)
        set_list.pop()
```
**Partitions**  
For partitioning is is important to create an *is_valid* method to make sure the segment we split fits the criteria of the given problem.  

Parameters:
* resulting list
* string
* partition
* start 
```python
def partitions(string):
    res = list()
    backtrack(res, string)
    return res
 
 
def backtrack(res, string, partition=list(), start=0):
    if len(partition) == 4:
        res.append(partition.copy())
    else:
        for i in range(start, len(string)):
            segment = string[start:i + 1]
            if is_valid(segment):
                partition.append(segment)
                backtrack(res, string, partition, i + 1)
                partition.pop()
 
 
def is_valid(segment):
    if len(segment) != 1:
        return False
    return True
```

---

## 4. Graph
### 4.1 Shortest Path
**Breath-first search** 
  
Data structure:
* Visited Nodes: Set
* Open Nodes: Queue

### 4.2 All Path and/or Length does not matter
**Depth-first search**  

Data Structure:  
* Visited Nodes: Set
* Open Nodes: Stack
  
**Combination formula**  
* Binominal Coefficient --> n! / n!m!

### 4.3 Local Maxima
**Hill Climbing**

---

## 5. Math Problems 
### Switch a and b without using extra space
* XOR, multiplication/division
* Bit Shifting 

---

## 6. Palindrome
### 6.1 Verify if palindrome  
1) Iterate Values and append them to the stack until halfway point
2. Keep iterating values and compare them to the popped element of the stack.   

---

## 7. Patterns
### 7.1 Create all valid patterns
* Recursion

---

## 8. String Manipulation
### 8.1 String Matching
* Dynamic Programming
* Tries

### 8.2 Unique Characters in a String
* Bit Manipulation --> 26 bit integer where every character is a letter in the alphabet

---

## 9. Updating + Adding/Removing elements  
Some problems require us to add/remove elements in O(1) time as well as updating them in O(1) time.  

**Create a(n) model/object of the values**  
  
**If FIFO problem**  
Use dictionary + queue where pointers to object is stored in both

**If LIFO problem**  
Use dictionary + stack where pointers to objects are stored in both.