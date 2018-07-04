# Study Guide
General Guidelines to solving interview problems

---

## 1. Arrays
### 1.1 Sorted
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
* Binary Search
* Convert to Graph problem 
    Try and see if the question uses an array to represent a graph.
* Dynamic Programming  
    Find recurring relationship on right and/or left sides of each elements

### 1.3 Unstructured and Unsorted
* Linear Search + Hashmap  
* Merge Sort
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
* Quick Sort
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
### 2.1 Inorder
* Visit left --> root --> right

### 2.2 PreOrder
* Visit root --> left --> right

### 2.3 PostOrder
* Visit left --> right --> root

---

## 3. String Manipulation
### 3.1 Permutation, SubSet, Combination, Partition
### Use Backtracking
**Combinations**  
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

### 3.2 String Matching
* Dynamic Programming
* Tries

### 3.3 Unique Characters in a String
* Bit Manipulation --> 26 bit integer where every character is a letter in the alphabet

---

## 4. Graph
### 4.1 Shortest Path
* Breath-first search

### 4.2 All Path and/or Length does not matter
* Depth-first search
* Combination formula

---

## 5. Math Problems 
### Switch a and b without using extra space
* XOR, multiplication/division
* Bit Shifting 

---

## 6. Pattern/Palindrom
### 6.1 Verify if each bracket has a closing bracket
* Iteration + Stack

### 6.2 Create all valid patterns
* Recursion
