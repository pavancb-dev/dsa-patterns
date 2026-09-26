## Prefix Sum + HashMap

### Recognition

Think Prefix Sum + HashMap when dealing with:

- Continuous subarrays
- Subarray sum equals K
- Counting subarrays satisfying a condition
- Negative numbers prevent normal sliding-window logic
- Need to relate current cumulative state to a previous state

### Core Derivation

For a subarray with sum `k`:

currentPrefix - previousPrefix = k

Rearrange:

previousPrefix = currentPrefix - k

Therefore, while processing the current prefix sum,
look for:

currentPrefix - k

among previously seen prefix sums.

### HashMap

Store:

prefixSum → frequency

Frequency is necessary because the same prefix sum may
occur multiple times.

Each occurrence can represent a different valid
subarray ending at the current position.

### Initialization

Start with:

0 → 1

This represents the empty prefix.

It allows subarrays beginning at index 0 to be counted
without special handling.

### Algorithm

prefix = 0
count = 0
freq = {0: 1}

For each number:

1. Add number to prefix
2. Compute `needed = prefix - k`
3. Add `freq[needed]` to answer
4. Increment `freq[prefix]`

### Invariant

Before processing the current prefix:

`freq[x]` = number of times prefix sum `x`
has occurred previously.

Therefore:

`freq[prefix - k]`

is the number of valid subarrays ending at the
current position.

### Complexity

Time: O(n) average
Space: O(n)

### Pitfalls

- Store prefix frequency, not just one index
- Initialize `{0: 1}`
- Query before adding the current prefix
- Don't default to sliding window when negatives exist
- Don't confuse prefix sums with individual array values
- Avoid naming a Python variable `sum`

### 10-Second Recall

Subarray sum = K

currentPrefix - previousPrefix = K
previousPrefix = currentPrefix - K

→ HashMap stores prefixSum → frequency
→ initialize 0 → 1
→ lookup before inserting current prefix.

### Python

```python
def subarraySum(nums: list[int], k: int) -> int:
    prefix_sum, count, freq = 0, 0, {0: 1}

    for num in nums:
        prefix_sum += num
        count += freq.get(prefix_sum - k, 0)
        freq[prefix_sum] = freq.get(prefix_sum, 0) + 1

    return count
