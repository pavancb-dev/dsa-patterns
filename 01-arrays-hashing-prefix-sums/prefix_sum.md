# Prefix Sum

## Recognition

Think Prefix Sum when dealing with:

- Range/subarray sums
- Multiple range queries
- Cumulative totals
- Need information about an interval
- Need to avoid recalculating ranges

## Core Idea

Store cumulative information.

For an array `nums`:

prefix[i] = sum of elements before index i

Use an array of size `n + 1`:

prefix[0] = 0

prefix[i + 1] = prefix[i] + nums[i]

## Range Sum

For inclusive range:

[left, right]

sum = prefix[right + 1] - prefix[left]

## Why It Works

prefix[right + 1]
contains everything through `right`.

prefix[left]
contains everything before `left`.

Subtracting cancels the unwanted prefix.

## Complexity

Build prefix:
Time: O(n)
Space: O(n)

Each range query:
Time: O(1)

For q queries:

O(n + q)

instead of:

O(q * n)

## Invariant

prefix[i] represents the sum of:

nums[0 ... i-1]

## Edge Cases

- left = 0
- right = n - 1
- single-element range
- negative numbers
- empty input
- integer overflow in languages like Java

## Common Pitfalls

- Off-by-one errors
- Confusing `prefix[i]` with sum through index `i`
- Forgetting `right + 1`
- Incorrect handling when left = 0
- Using `int` when cumulative sum can overflow

## 10-Second Recall

Need repeated range sums
→ cumulative sum
→ prefix has n + 1 elements
→ prefix[i] = sum before i
→ range(L,R) = prefix[R+1] - prefix[L].
