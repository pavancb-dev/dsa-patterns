## Two Sum

**Pattern:** Hash Map — Complement Lookup

### Recognition

Need to find two values satisfying:

a + b = target

Rearrange:

b = target - a

For each number, check whether its required complement
has already been seen.

### Approach

Maintain:

number -> index

For each `nums[i]`:

1. `complement = target - nums[i]`
2. Check if complement exists in `seen`
3. If yes, return its index and `i`
4. Otherwise store `nums[i] -> i`

### Invariant

Before processing index `i`, `seen` contains only
elements from indices `< i`.

This guarantees we don't use the same index twice.

### Complexity

Time:  O(n) average
Space: O(n)

### Pitfalls

- Store `number -> index`, not `index -> number`
- Lookup before inserting current element
- Duplicates are valid: `[3,3]`, target `6`
- Don't confuse the complement with an index

### Python

```python
def twoSum(nums, target):
    seen = {}

    for i, num in enumerate(nums):
        complement = target - num

        if complement in seen:
            return [seen[complement], i]

        seen[num] = i

    return [-1, -1]
