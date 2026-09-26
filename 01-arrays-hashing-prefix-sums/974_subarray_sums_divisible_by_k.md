## Prefix Sum + Modulo

### Recognition

Think Prefix Sum + Modulo when dealing with:

- Continuous subarrays
- Divisibility by K
- Sum % K conditions
- Need to count subarrays satisfying modular conditions

### Core Derivation

Subarray sum:

currentPrefix - previousPrefix

For it to be divisible by K:

(currentPrefix - previousPrefix) % K = 0

This happens when:

currentPrefix % K == previousPrefix % K

Therefore:

Equal prefix remainders identify a subarray
whose sum is divisible by K.

### HashMap

Store:

remainder → frequency

If the current remainder has appeared `x` times,
there are `x` valid subarrays ending here.

### Initialization

{0: 1}

The empty prefix has remainder 0.

This allows a prefix whose entire sum is divisible by K
to be counted naturally.

### Algorithm

prefix = 0
count = 0
freq = {0: 1}

For each number:

1. prefix += number
2. remainder = prefix % K
3. count += freq[remainder]
4. increment freq[remainder]

### Complexity

Time: O(n)
Space: O(min(n, K))

### Negative Remainders

Python:

-2 % 5 == 3

Python already normalizes to a non-negative remainder.

Java:

-2 % 5 == -2

Normalize when necessary:

((prefix % k) + k) % k

### Pitfalls

- Store remainder frequency, not prefix frequency
- Initialize remainder 0 with frequency 1
- Query before inserting current remainder
- Count every previous matching remainder
- Be careful with negative modulo semantics across languages

### 10-Second Recall

Subarray divisible by K
→ difference of two prefixes divisible by K
→ their remainders must match
→ store remainder → frequency
→ matching remainder count = new valid subarrays.

### Python 

```python
def subarrays_div_by_k(nums: list[int], k: int) -> int:
    prefix_sum, count, freq = 0, 0, {0: 1}

    for num in nums:
        prefix_sum += num
        key = prefix_sum % k
        count += freq.get(key, 0)
        freq[key] = freq.get(key, 0) + 1

    return count
