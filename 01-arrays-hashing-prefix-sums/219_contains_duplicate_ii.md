## Contains Duplicate II

**Pattern:** HashMap — Last Seen Index

### Recognition

Need to determine whether the same value occurs
within a maximum index distance `k`.

The value alone is insufficient.
We need:

number → most recent index

### Approach

Process the array left-to-right.

For each `nums[i]`:

1. Check whether the number was seen before.
2. If yes, calculate the distance from its most recent index.
3. If `i - previousIndex <= k`, return true.
4. Update the map with the current index.

### Why Most Recent Index?

For the current index `i`, the most recent previous
occurrence gives the minimum possible index distance.

### Invariant

Before processing index `i`:

`seen[num]` stores the most recent index
where `num` appeared among indices `< i`.

### Complexity

Time: O(n) average
Space: O(n)

### Pitfalls

- HashSet cannot provide the previous index
- Store the most recent occurrence, not the first
- Current index must not be stored before checking
- `abs()` is unnecessary when scanning left-to-right

### 10-Second Recall

Need same value + nearby index
→ HashMap
→ number → latest index
→ check `i - previousIndex <= k`
→ update latest index.

### Python

```python
def containsNearbyDuplicate(nums: list[int], k: int) -> bool:
    seen = {}
    for i, num in enumerate(nums):
        if num in seen and i - seen[num] <= k:
            return True
        seen[num] = i
    return False
