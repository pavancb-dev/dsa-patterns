# Bucket Sort — Bounded Key / Frequency

## Recognition

Consider buckets when:

- Objects need to be ordered/grouped by some numeric property
- That property has a small or bounded range
- General comparison sorting would be unnecessary

Ask:

"What is the possible range of the value I'm sorting by?"

## Core Idea

Instead of comparison sorting:

items.sort(...)

create buckets where the index represents the sorting property.

bucket[property] → items having that property

Then traverse buckets in the required order.

## Top K Frequent Example

Input:

[1,1,1,2,2,3]

Frequency map:

1 → 3
2 → 2
3 → 1

Since an array of length n allows frequencies only from:

1..n

create:

bucket[frequency] → numbers

Result:

bucket[1] → [3]
bucket[2] → [2]
bucket[3] → [1]

Scan buckets from n → 1.

First k numbers encountered are the k most frequent.

## Why n + 1 Buckets?

Maximum possible frequency = n.

We want indices:

0, 1, 2, ..., n

Therefore:

buckets = [[] for _ in range(n + 1)]

## Complexity

For Top K Frequent:

Frequency counting: O(n)
Building buckets:   O(n)
Scanning buckets:   O(n)

Total: O(n)

Space: O(n)

## Pitfalls

- Bucket index must have a known/manageable range
- Multiple elements may have the same frequency
- Each bucket therefore stores a collection, not one value
- Stop once k results are collected
- Don't sort the buckets afterward—that defeats the purpose
- `buckets[::-1]` creates a copy in Python; `reversed()` does not

## 10-Second Recall

Need ordering by bounded numeric property
→ property becomes bucket index
→ bucket[index] stores matching items
→ traverse buckets in desired order.

### Python
```python
def topKFrequent(nums: list[int], k: int) -> list[int]:
    freq = Counter(nums)
    buckets = [[] for _ in range(len(nums) + 1)]
    for num, count in freq.items():
        buckets[count].append(num)
    res = []
    for bucket in reversed(buckets):
        for num in bucket:
            res.append(num)
            if len(res) == k:
                return res
    return res
