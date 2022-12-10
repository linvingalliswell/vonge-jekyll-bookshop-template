---
title: Minimum Swaps to Sort
date: 2022-12-07 14:45 +03:00
image: '/images/post-3.jpg'
tags: [small-algo, sort]
---

## Let's Sort

This post will introduce the easiest way, in my opinion, to count the minimum number of swaps to sort an array. The algorithm generally runs in O(NlogN) time, dominated by the complexity of the sorting algorithm. It will, however, cost us additional O(n) space for the sorted array and original indexes to be compared (plus space used in sorting of course).

Note that the method should only be applied when the elements in the array are unique, and it thus disregards the stability of sorting.

```python
# python 3.10

def min_swap_to_sort(arr):
    indexes = {x: i for i, x in enumerate(arr)}

    out = 0
    for x, y in zip(arr, sorted(arr)):
        if x != y:
            arr[indexes[x]], arr[indexes[y]] = y, x
            indexes[x], indexes[y] = indexes[y], indexes[x]
            out += 1
    return out
```

\\
I first create a hashmap of value and index pairs and a sorted version of the original array. Then, zip out two arrays and compare the elements; if the element is not in the right position, I make a swap, update the indexes, and increment the counter. The process is straightforward.

---

### Relevant Questions

-   [2471. Minimum Number of Operations to Sort a Binary Tree by Level](https://leetcode.com/problems/minimum-number-of-operations-to-sort-a-binary-tree-by-level/ 'LeetCode 2471. Minimum Number of Operations to Sort a Binary Tree by Level')
