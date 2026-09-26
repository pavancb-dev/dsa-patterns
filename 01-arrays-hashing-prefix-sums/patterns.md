# Arrays, Hashing & Prefix Sums — Pattern Notes

> Goal: a fast-glance sheet for recognizing patterns, recalling the invariant, avoiding common traps, and rebuilding the solution from first principles.

---

## 1. Hash Set / Hash Map Lookup

### When to recognize it

Use hashing when the problem repeatedly asks questions like:

- Have I seen this value before?
- Does a matching/complementing value exist?
- What index belongs to this value?
- How many times has this value appeared?
- Can I group elements by a key?

### Core idea

Replace repeated linear scans with average O(1) lookup by storing information about elements already processed.

### Mental model

**Scan once -> store useful state -> query the state.**

### Typical state

```text
Set:       value -> presence
Map:       value -> index / count / aggregate information
Map:       key   -> list/group of values
```

### Generic template

```text
state = empty map/set

for x in array:
    if state contains what I need:
        use/update answer
    update state with x
```

### Edge cases

- Duplicate values may be meaningful; do not accidentally overwrite useful information.
- The problem may care about the **first** index, **last** index, or **count**. Choose the map value accordingly.
- Decide whether the current element is allowed to match itself before inserting it.
- Empty input and one-element input often need no special code if the invariant is set up correctly.

### Pitfalls

1. **Insert-before-check vs check-before-insert**
   - Two Sum usually needs `check complement -> then insert current` when using indices, so the same element is not reused.

2. **Storing the wrong occurrence**
   - Longest-distance/longest-subarray problems often want the earliest index.
   - Latest-occurrence problems need the latest index.

3. **Assuming O(1) is guaranteed**
   - Hash tables are typically O(1) average-case, not a mathematical worst-case guarantee.

4. **Forgetting key normalization**
   - For grouping/counting, equivalent inputs may need to map to the same key.

### Complexity

- Average lookup/insert: O(1)
- Full scan: usually O(n)
- Extra space: usually O(n)

### Fast-glance trigger

> **If I am repeatedly searching the array for something I have already seen, think HASHING first.**

---

## 2. Frequency Counting

### When to recognize it

Use frequency maps when the question is fundamentally about **how often** something occurs.

Common signals:

- anagram/permutation checks
- duplicates
- character/number counts
- most/least frequent element
- grouping equal signatures
- comparing two collections by multiplicity

### Core idea

Represent a collection as:

```text
item -> frequency
```

Then compare or reason about counts instead of individual positions.

### Generic template

```text
freq = empty map

for x in data:
    freq[x] += 1
```

For two collections:

```text
for x in first:
    freq[x] += 1

for x in second:
    freq[x] -= 1
```

Then verify all relevant counts return to zero.

### Edge cases

- Different lengths can often be rejected immediately.
- Negative/zero frequencies after comparison indicate mismatch.
- If the input domain is tiny (for example lowercase English letters), an array can replace a hash map.

### Pitfalls

- Using a map when a fixed-size array would be simpler and faster.
- Forgetting that multiplicity matters: `{a, a, b}` is not the same as `{a, b}`.
- Building a frequency map but then doing another O(n) scan for every key, accidentally creating O(n²).

### Complexity

Typically O(n) time and O(k) space, where `k` is the number of distinct values.

### Fast-glance trigger

> **If order does not matter but counts do, think FREQUENCY MAP.**

---

## 3. Two Sum / Complement Lookup

### When to recognize it

A problem gives a target and asks for two elements whose relationship is:

```text
x + y = target
```

or a similar equation where, after choosing `x`, the required `y` can be computed.

### Core idea

For each current value `x`, compute:

```text
needed = target - x
```

Then ask whether `needed` has already been seen.

### Invariant

At index `i`, the data structure contains exactly the information from the portion of the array that is already processed.

### Template

```text
seen = empty map   // value -> index

for i from 0 to n-1:
    needed = target - nums[i]

    if needed in seen:
        return [seen[needed], i]

    seen[nums[i]] = i
```

### Edge cases

- Duplicate values can form the answer: `[3, 3]`, target `6`.
- Negative values are fine.
- Target or values may be zero.
- The same element cannot be used twice unless the problem explicitly allows reuse.

### Pitfalls

- Checking after inserting the current value can incorrectly match an element with itself.
- If duplicates exist, decide whether you need the first/last index or just any valid pair.
- Integer overflow can matter when arithmetic uses fixed-width integer types and constraints are large. Use a wider type where required.

### Complexity

- Time: O(n) average
- Space: O(n)

### Fast-glance trigger

> **Can I calculate the exact value I need from the current value? -> COMPLEMENT HASH MAP.**

---

## 4. Prefix Sum

### When to recognize it

Use prefix sums when the problem asks for repeated sums over contiguous ranges or when a subarray sum can be expressed using two prefix values.

### Core idea

Define:

```text
prefix[i] = sum of elements before index i
```

The safest convention is often a prefix array of length `n + 1`:

```text
prefix[0] = 0
prefix[i + 1] = prefix[i] + nums[i]
```

Then sum of `nums[l..r]` is:

```text
prefix[r + 1] - prefix[l]
```

### Why it works

The prefix before `r + 1` contains everything through `r`.
Subtracting the prefix before `l` removes everything before `l`.

### Example

```text
nums   = [2, 4, -1, 3]
prefix = [0, 2,  6,  5, 8]

sum(1..3) = prefix[4] - prefix[1]
          = 8 - 2
          = 6
```

### Edge cases

- Range starts at index `0`.
- Range ends at the final index.
- Empty range behavior must match the problem's definition.
- Negative values do not break prefix sums.
- Use a sufficiently wide numeric type for potentially large totals.

### Pitfalls

1. **Off-by-one errors**
   - Decide whether prefix index `i` means “through `i`” or “before `i`”.
   - The `n + 1` convention greatly reduces boundary mistakes.

2. **Overflow**
   - Even if each element fits in 32-bit integer range, the total sum may not.

3. **Unnecessary prefix array**
   - If there is only one traversal and no future range queries, a running sum may be enough.

### Complexity

- Build: O(n)
- Each range query: O(1)
- Space: O(n)

### Fast-glance trigger

> **Repeated contiguous-range sums -> PREFIX SUM.**

---

## 5. Prefix Sum + Hash Map

### When to recognize it

This is one of the highest-value patterns in this section.

Think of it when the problem asks for:

- number of subarrays with sum = `k`
- longest subarray with sum = `k`
- whether any subarray has sum = `k`
- subarrays whose sum satisfies a simple prefix relationship

### Core transformation

Suppose current prefix sum is `P` and some earlier prefix sum is `Q`.

The sum of the subarray between them is:

```text
P - Q
```

To make that sum equal `k`:

```text
P - Q = k
Q = P - k
```

So while scanning, the question becomes:

> **Have I seen prefix sum `P - k` before?**

### Count subarrays with sum k

Store how many times each prefix sum has appeared.

```text
count = {0: 1}
prefix = 0
answer = 0

for x in nums:
    prefix += x
    answer += count.get(prefix - k, 0)
    count[prefix] = count.get(prefix, 0) + 1
```

### Why initialize `{0: 1}`?

It represents an empty prefix before the array starts.

Without it, a valid subarray beginning at index `0` would be missed.

### Longest subarray with sum k

Store the **first index** at which each prefix sum appears.

```text
first = {0: -1}
prefix = 0
best = 0

for i in 0..n-1:
    prefix += nums[i]

    if (prefix - k) in first:
        best = max(best, i - first[prefix - k])

    if prefix not in first:
        first[prefix] = i
```

### Critical invariant

For index `i` with prefix `P`, every previous prefix equal to `P - k` defines a valid subarray ending at `i`.

### Edge cases

- Subarray starts at index `0`.
- Negative values are allowed.
- Zero values can create many repeated prefix sums.
- `k = 0` is especially important: repeated prefix sums correspond to zero-sum subarrays.
- For counting problems, duplicate prefix sums must be counted, not overwritten.

### Pitfalls

1. **Using a Set instead of a frequency Map for counting**
   - One matching prefix is not enough; multiple earlier matches create multiple valid subarrays.

2. **Overwriting first index in longest-subarray problems**
   - Keep the earliest occurrence to maximize length.

3. **Initializing the wrong base state**
   - `{0: 1}` for counting.
   - `{0: -1}` for longest-length indexing.

4. **Assuming positive numbers**
   - Prefix-sum + hash-map methods are valuable precisely because they work with negatives too.

### Complexity

- Time: O(n) average
- Space: O(n)

### Fast-glance trigger

> **Subarray + exact sum + negatives possible -> PREFIX SUM + HASH MAP.**

---

## 6. Prefix / Suffix Running State

### When to recognize it

Use when each position needs information about everything to its left, everything to its right, or both.

Examples of state:

- prefix maximum/minimum
- prefix product/sum
- suffix maximum/minimum
- count of something before/after the current index

### Core idea

Precompute or maintain aggregate state so each element does not rescan the array.

### Two-pass form

```text
leftState[i]  = aggregate of elements before i
rightState[i] = aggregate of elements after i
```

Then combine them at each position.

### Space optimization

Often one side can be stored in the output while the other side is maintained as a running variable.

### Edge cases

- First element has no left-side data.
- Last element has no right-side data.
- Decide the identity value for the aggregate (for example `0` for sum, `1` for product, negative/positive infinity where appropriate).

### Pitfalls

- Accidentally including the current element when the problem excludes it.
- Using an invalid identity value.
- Building both arrays when O(1) auxiliary space is possible.

### Fast-glance trigger

> **“Everything before/after this index” -> PREFIX/SUFFIX STATE.**

---

## 7. Difference Array

### When to recognize it

Use when there are many range update operations such as:

```text
add value x to every position in [l, r]
```

and you do not need the fully updated array after every single operation.

### Core idea

Instead of updating every element in `[l, r]`, record only where the change starts and where it stops.

For an update of `+x` on `[l, r]`:

```text
diff[l] += x
diff[r + 1] -= x
```

Then take a prefix sum over `diff` to reconstruct the final values.

### Edge cases

- Update ends at `n - 1`, so `r + 1` may equal `n`.
- You often need an array of length `n + 1` for the marker at `r + 1`.
- Multiple updates accumulate naturally.

### Pitfalls

- Forgetting the `r + 1` subtraction.
- Using the pattern when intermediate states after every query are required.
- Confusing difference arrays with prefix sums: difference array is about **fast updates**, prefix sums are about **fast queries**.

### Complexity

- q range updates: O(q)
- Reconstruct final array: O(n)
- Total: O(n + q)

### Fast-glance trigger

> **Many range additions + final result needed -> DIFFERENCE ARRAY.**

---

## 8. In-place Array Marking

### When to recognize it

Use when values lie in a constrained range tied to indices, such as values in `[1, n]`, and the problem asks for missing/duplicate/seen information while limiting extra space.

### Core idea

Reuse the input array itself as a state structure.

Common technique:

- map value `v` to index `v - 1`
- negate/modify the value at that index to mark “seen”
- inspect the final state to find missing or duplicated values

### Edge cases

- Values outside the expected range.
- Value `0`, if present, may require a different mapping.
- Already-modified entries must still allow you to recover the original logical value.

### Pitfalls

- Modifying input when the problem forbids it.
- Losing the original value before computing its mapped index.
- Sign/absolute-value mistakes when entries have already been negated.

### Complexity

Usually O(n) time and O(1) extra space.

### Fast-glance trigger

> **Values map naturally to array indices + O(1) extra space constraint -> IN-PLACE MARKING.**

---

# Common Edge Cases Across This Section

Before submitting, mentally test:

```text
[]
[1]
[0]
[0, 0]
[-1, 1]
[1, -1, 1, -1]
large positive values
large negative values
duplicates
all values equal
answer uses the first element
answer uses the last element
answer spans the entire array
no valid answer
multiple valid answers
```

Also ask:

- Can arithmetic overflow?
- Does the problem permit modifying the input?
- Do I need indices or only values?
- Do I need any match, the first match, the last match, the longest, or the count of all matches?
- Are negative numbers present? If yes, sliding-window assumptions often break; reconsider prefix sums/hash maps.

---

# Pattern Selection Cheat Sheet

| Problem wording / signal | First pattern to consider |
|---|---|
| “Have we seen this before?” | Hash Set / Map |
| “Count occurrences / frequency” | Frequency Map |
| “Find two values that…” | Complement Hash Map |
| “Sum from L to R” | Prefix Sum |
| “How many subarrays sum to K?” | Prefix Sum + Frequency Map |
| “Longest subarray sum K” | Prefix Sum + Earliest-Index Map |
| “Everything left/right of i” | Prefix/Suffix State |
| “Many updates on ranges” | Difference Array |
| “O(1) extra space, values 1..n” | In-place Marking |

---

# Mistake Log

Use this section after solving problems. Do not only record the final solution; record the reason your first approach failed.

### Mistake categories

- Pattern not recognized
- Wrong invariant
- Off-by-one
- Incorrect initialization
- Duplicate handling
- Overflow
- Wrong data structure
- Time complexity too high
- Space complexity too high
- Edge case missed
- Implementation bug
