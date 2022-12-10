---
title: Maximum Number of Non-overlapping Palindrome Substrings
date: 2022-12-07 14:45 +03:00
image: '/images/post-2.jpg'
tags: [leetcode, palindrome]
---

## LeetCode 2472

In this post, I will talk about the solution to the LeetCode problem [2471. Minimum Number of Operations to Sort a Binary Tree by Level](https://leetcode.com/problems/maximum-number-of-non-overlapping-palindrome-substrings/description/ 'LeetCode 2472. Maximum Number of Non-overlapping Palindrome Substrings').

## Solution

```python
# python 3.10

def max_palindromes(s: str, k: int) -> int:
    n = len(s)

    # i, j palindrome, inclusive
    is_pal = [[True] * n for _ in range(n)]
    out = n
    for len_ in range(2, k+2):
        for i in range(n-len_+1):
            j = i + len_ - 1
            is_pal[i][j] = is_pal[i+1][j-1] and s[i] == s[j]
            out += is_pal[i][j]

    i = 0
    out = 0
    while i < n:
        # only need to consider k and k+1 length
        if (i+k-1 < n and is_pal[i][i+k-1]) or (i+k < n and is_pal[i][i+k]):
            out += 1
            # move to t, where t is the tail of a palindrome
            i += k - (1 if is_pal[i][i+k-1] else 0)
        i += 1
    return out

```

\\
The intuition is to use a greedy algorithm starting from index 0 and iterate forward; the shortest substring candidate is our best shot among all the substrings that are at least k length starting at index i, because it will give us the longest remaining string for our finding onward, namely, it will grant us the best chance to find another valid substring and thus yield a better answer.

Also, if we can find a valid substring at index i, there is no need to search for the substring starting index (i+1) and potentially sacrifice our current substring at index i. This is because the optimal substring length is either k or (k+1), which is another important observation for optimization.

---

## Why Search for k and k+1 Only

An interesting property of palindrome: If a string of (k+2) length is a palindrome, its substring of k length will be a palindrome too, as shown below.

Given any palindrome whose composition can be expressed with characters A, B, and C of any choice (they can be the same) and starts at index 0.

If (k+2) is odd, say 5, and we have a (k+2) length palindrome whose composition is CBABC, we will have a k length palindrome, BAB. Although we can’t count it at index 0, it will certainly be included in the next iteration of index 1.

If (k+2) is even, say 4, and we have a (k+2) length palindrome whose composition is BCCB, we can apply the same procedure to get a palindrome of k, CC at index 1.

Learning from the deduction above, we can reduce the search at index i to the palindromes of k and (k+1) length only; we can reduce k+2n length palindrome to k and k+2n+1 length palindrome to (k+1) where n is a constant

## Other Techniques

I use the same technique for [Palindromic Substrings](/blog/palindromic-substrings) to optimize for the validity query of palindrome; here we choose to use tabulation for simplicity. We also prune the operation and move the next step to t+1 when we find a valid substring ending at t

## Summary

I guess the inspiration here is that (k+2) length palindromes can be reduced to k length, a property that might be found useful in the future.

---

### Relevant Questions

-   [2471. Minimum Number of Operations to Sort a Binary Tree by Level](https://leetcode.com/problems/maximum-number-of-non-overlapping-palindrome-substrings/description/ 'LeetCode 2472. Maximum Number of Non-overlapping Palindrome Substrings')
-   [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/description/ 'LeetCode 647. Palindromic Substrings')
