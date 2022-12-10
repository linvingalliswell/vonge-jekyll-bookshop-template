---
title: Reverse a Singly Linked List
date: 2022-12-05 14:45 +03:00
image: '/images/post-1.jpg'
tags: [small-algo, linked-list]
---

## Reversing the List

This post will reveal how to reverse a singly linked list with constant space, which is also in-place as we will modify the original list.

```python
# python 3.10

from dataclasses import dataclass

@dataclass
class Node:
    val: int
    next_: Node = None

def reverse_list_inplace(node: Node) -> None:
    prev = None
    while node:
        tmp = node.next_
        node.next_ = prev
        prev = node
        node = tmp
```

\\
I first use a temporary pointer `tmp` to hold the reference to the next node, modify the `next` pointer of the current node to hold the reference to the previous node instead of the next node, update `prev` pointer, and finally move to the original next node stored in the `tmp` pointer. I continue the process until reaching the end of the linked list.

Here I only need to allocate memory for two additional pointers; one for reference to the previous node, and one for the next node.

---

## Backward Traversal

The trick can help us traverse the singly linked list backward. Unlike the doubly linked list where we can move the iterator to the tail of the list and traverse backward with the member pointer, the singly linked list only has a member pointer to the next node. One way to achieve the same behavior with the singly linked list is to use a stack; traverse the linked list, store the values in a stack on the fly, and finally pop them out sequentially. However, this will cost us O(n) extra space to hold the values in the stack.

If we reverse the linked list, and then traverse backward while doing a second reversal, we can eventually get the same result with a singly linked list without introducing linear space.

```python
def print_list_backward(node: Node) -> None:
    prev = None
    while node:
        tmp = node.next_
        node.next_ = prev
        prev = node
        node = tmp

    node = prev
    prev = None
    while node:
        print(node)
        # reverse the list again
        # to keep the input list unchanged
        tmp = node.next_
        node.next_ = prev
        prev = node
        node = tmp

```

---

### Relevant Questions

-   [2487. Remove Nodes From Linked List](https://leetcode.com/problems/remove-nodes-from-linked-list/ 'LeetCode 2487. Remove Nodes From Linked List')
