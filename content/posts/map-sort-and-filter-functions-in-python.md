---
title: "Map, Sort, and Filter Functions in Python"
date: 2022-04-23T23:03:25+04:00
draft: false
toc: false
images:
tags:
  - Python
---

This tutorial will help you to see real example of using map, sort, and filter functions along with lambda functions in Python.

**Map Function Example**

Python's map() is a built-in function that allows you to process and transform all the items in an iterable without using an explicit for loop, a technique commonly known as mapping. map() is useful when you need to apply a transformation function to each item in an iterable and transform them into a new iterable.

```py
    target_list = [1, 2, 3, 4, 5, 6] 
    func = lambda element: element ** 2
    print(list(map(func, target_list)))
```

**Sort Function Example**

The sort() method sorts the elements of an array in place and returns the sorted array. The default sort order is ascending, built upon converting the elements into strings, then comparing their sequences of UTF-16 code units values.

```py
    users = [("Yalchin", "age", 20), ("Coder", "age", 19)]
    func = lambda user: user[2]
    users.sort(key=func,reverse=True)
    print(users)
```

**Filter Function Example**

The filter() method constructs an iterator from elements of an iterable for which a function returns true. In simple words, filter() method filters the given iterable with the help of a function that tests each element in the iterable to be true or not.

```py
    ages = [18, 33, 3, 10, 9, 23]
    func = lambda age: age > 18
    x = filter(func, ages)
    print(list(x))
```