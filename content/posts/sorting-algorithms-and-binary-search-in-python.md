---
title: "Sorting Algorithms and Binary Search in Python"
date: 2022-04-24T14:20:57+04:00
draft: false
toc: false
cover: "blog-images/sort_python.png"
useRelativeCover: true
images:
tags:
  - Python
  - Algorithms
---



### **Selection Sort**


Selection sort is about selecting the minimum value in an unsorted array and moving it towards the front by comparing. Here is an example code snippet to implement binary search in Python:

```py
    def selection_sort(list_):
        
        length = len(list_)
        for index in range(length - 1):
            min = list_[index]
    
            for j in range(index+ 1, length):
                if list_[j] < min:
                    min = list_[j]
                    list_[j], list_[index] = list_[index], list_[j]
            
        return list_
    
    unsorted_list = [7,8,9,8,7,6,5,6,7,8,9,8,7,6,5,6,7,8,0]
    selection_sort(unsorted_list)
```
---
### **Buble Sort**


The bubble sort algorithm is applied by going through an array of data a number of times and at the same time comparing two adjacent numbers at a time in order to reorder them if there are out of order. Here is an example snippet written in Python:

```py
    def buble_sort(list_):
        boundary = len(list_) - 1
    
        for i in range(boundary):
            if list_[i] > list_[i+1]:
                list_[i+1], list_[i] = list_[i] ,list_[i+1]
    
        return list_
    
    buble_sort(unsorted_list)
```
---
### **Insertion Sort**

Insertion Sort algorithm is applied by going through an unsorted list, comparing the current value with the previous one, and in case the previous number is greater we swap them. Here is the code implementation of the insertion sort algorithm in Python:

```py
    def insertion_sort(list_a):
        indexing_length = range(1, len(list_a))
        for i in indexing_length:
            value_to_sort = list_a[i]
    
            while list_a[i-1] > value_to_sort and i>0:
                list_a[i], list_a[i-1] = list_a[i-1], list_a[i]
                
        return list_a
    
    print(insertion_sort(unsorted_list))
```
---

### **Quicksort**

Quick Sort algorithm is applied by choosing a pivot element from an unsorted list and dividing the others into two sub-lists, which we can call less and greater than elements of arrays in comparison to chosen pivot element. Here is the code implementation of quick sort algorithm written in Python: 

```py
    def quick_sort(list_):
        len_ = len(list_)
        if len_ <= 1:
            return list_
        else:
            pivot = list_.pop()
    
        l_greater = []
        l_lower = []
    
        for element in list_:
            if element > pivot:
                l_greater.append(element)
    
            else:
                l_lower.append(element)
    
        return quick_sort(l_lower) + [pivot] + quick_sort(l_greater)
     
    print(quick_sort(unsorted_list))
```

Quicksort algorithm is considered to be the fastest sorting algorithm as the time complexity of Quicksort is O(n log n) in the best case, O(n log n) in the average case, and O(n^2) in the worst case. But because it has the best performance in the average case for most inputs, Quicksort is generally considered the “fastest” sorting algorithm

---

### **Binary Search**

The binary search algorithm is applied by finding the middle point of the list and checking if the target number is greater or less than the selected middle point. As only one of these cases will be True, we will find a new middle point and divide the list till the middle point is our target. Here is the code implementation of the binary search algorithm:

```py
    def binary_search(sorted_list, target):
        len_ = len(sorted_list)
        start = 0
        end = len_ - 1
    
    
        while start <= end:
            mid_index = (start + end) // 2
            print(mid_index)
            mid_ = sorted_list[mid_index]
            print(mid_, sorted_list.index(mid_))
            
            if mid_ == target:
                return True, f"at {sorted_list[mid_index]}"
            
            elif target < mid_:
                end = mid_index - 1
    
            else:
                start = mid_index + 1
                
        return None

    sorted_list = insertion_sort(unsorted_list)
    target = int(input("Please enter the target: "))
    print(binary_search(sorted_list, target))
```

Binary search is more efficient than the linear search; it has a time complexity of O(log n). The list of data must be in sorted order for it to work. ... Its time complexity of O(log n) makes it very fast as compared to other sorting algorithms.

---
Thanks for reading!