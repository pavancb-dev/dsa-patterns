## Longest Consecutive Sequence

**Pattern:** HashSet — Sequence Start Detection

### Recognition

Need to find the longest consecutive sequence in an
unsorted array in O(n).

### Core Idea

Put all values into a HashSet for O(1) average lookup.

A number `x` is the start of a consecutive sequence only if:

`x - 1` is not in the set.

Only expand sequences from their starts.

### Algorithm

For every unique number `x`:

1. Check whether `x - 1` exists.
2. If it exists, `x` is not a sequence start → skip.
3. Otherwise repeatedly check:
   `x + 1`, `x + 2`, ...
4. Track maximum length.

### Key Insight

Do NOT expand from every number.

Only expand from sequence starts.

This prevents repeatedly traversing the same sequence.

### Complexity

Time: O(n) average
Space: O(n)

Although there is a nested while loop, the total number
of successful sequence-expansion steps across all starts
is O(n).

### Pitfalls

- Do not sort if O(n) is required
- Check `x - 1` to identify sequence starts
- Iterate over the set to avoid duplicate outer-loop work
- Don't assume nested loops automatically mean O(n²)

### 10-Second Recall

Longest consecutive sequence
→ HashSet
→ find numbers with no predecessor
→ expand only from sequence starts
→ O(n) average.

### Python

```python
def longestConsecutive(nums: list[int]) -> int:
    seen = set(nums)
    output = 0
    for num in seen:
        if num - 1 not in seen:
            count = 1
            while num + count in seen:
                count += 1
            output = max(output, count)
    return output
