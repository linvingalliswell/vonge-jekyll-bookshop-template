---
title: Palindromic Substrings
date: 2022-12-07 14:45 +03:00
image: '/images/post-4.jpg'
tags: [leetcode, palindrome]
---

## LeetCode 647

In this post, I will talk about the 3 different approaches to solve the LeetCode problem [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/description/ 'LeetCode 647. Palindromic Substrings'). The first two solutions are memoization and tabulation, which in my opinion, is more general in their usage, and the last one is quite specific to the problem, being optimized for space.

## Solution

The naive solution is to brute force enumeration of substrings, which takes O(n^2) time, and conduct a palindrome test on each of them. To optimize for time, we have two solutions in dynamic programming

## Tabulation

```python
# python 3.10

def count_substrings(s: str) -> int:
    n = len(s)
    dp = [[True] * n for _ in range(n)]
    out = n
    for len_ in range(2, n+1):
        for i in range(n-len_+1):
            j = i + len_ -1
            dp[i][j] = dp[i+1][j-1] and s[i] == s[j]
            out += dp[i][j]
    return out
```

\\
Because there is a relationship of the palindrome test between the substring from index i to j and the substring from (i+1) to (j-1), we can populate the table starting working on substrings with the length of 2, then move to substrings with the length of 3, and so on. The base case is when i == j, where the length of substrings is 1.

## Memoization

```python
from functools import cache

def count_substrings(s: str) -> int:
    @cache
    def is_pal(i: int, j: int) -> bool:
        if i >= j:
            return True
        return is_pal(i+1, j-1) and s[i] == s[j]

    n = len(s)
    out = 0
    for i in range(n):
        for j in range(i, n):
            out += is_pal(i, j)
    return out
```

\\
Similar to tabulation, we can also have the recursive function to handle the recurrence relation of the substrings and cache the result.

## Center Window

```python
def count_substrings(s: str) -> int:
    def count_pal(i: int, j: int) -> bool:
        while i >= 0 and j < n and s[i] == s[j]:
            i -= 1
            j += 1
        return (j - i) // 2

    n = len(s)
    out = 0
    for c in range(n):
        # odd length palindrome
        out += count_pal(c, c)
        # even length palindrome
        out += count_pal(c, c+1)
    return out
```

\\
Once we finish processing substrings from index (i+1) to (j-1) and move on to substrings from index i to j, we can discard the table of the former since it is no longer useful. Therefore, to optimize the space used, we can actually count the valid substrings on the fly and throw away the intermediate results.
We start by traversing the center of the possible palindromes, iterate the possible length of the substrings, beginning with the length of 1 and 2, which is to simply expand the head and tail of the substrings. We break the iteration if the substring is not palindrome because its superstrings will not be a palindrome either.

## Summary

Personally I prefer the dynamic programming method, because it is more general and the recurrence is more straightforward. For example, If I ever need to do a [quick query](/blog/maximum-number-of-non-overlapping-palindrome-substrings) with the head and tail indexes to determine whether a substring is a palindrome, the cache constructed from the first two methods will come in handy. The last method gives us excellent space complexity, O(1), because it knows that the intermediate results are irrelevant to this particular question. Overall, all three methods are easy enough to come up with in a 45-minute interview. I know there is a better way to do it with Manacher's algorithm, but because it is harder to understand, I decided to leave it to the next time.

---

### Relevant Questions

-   [2471. Minimum Number of Operations to Sort a Binary Tree by Level](https://leetcode.com/problems/maximum-number-of-non-overlapping-palindrome-substrings/description/ 'LeetCode 2472. Maximum Number of Non-overlapping Palindrome Substrings')
-   [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/description/ 'LeetCode 647. Palindromic Substrings')
