## Valid Anagram

**Pattern:** HashMap — Frequency Counting

### Recognition

Anagram requires the same characters with the same frequencies.

Existence alone is insufficient:

"aab" and "abb" both contain {a,b},
but their frequencies differ.

Therefore:

character → frequency

### Approach

Use one frequency-difference map.

For each position:

- increment frequency for `s[i]`
- decrement frequency for `t[i]`

At the end every frequency must equal 0.

### Invariant

`freq[c]` represents:

count of c seen in s - count of c seen in t

### Complexity

Time: O(n)
Space: O(k), where k = number of distinct characters
Worst case: O(n)

### Pitfalls

- Check string lengths first
- HashSet is insufficient because frequency matters
- Frequencies may be non-zero during traversal
- All frequencies must be zero only at the end

### 10-Second Recall

Same elements + same counts
→ frequency map
→ increment one input, decrement the other
→ everything should cancel to zero.

### Python

```python
def valid_anagram(s: str, t: str):
    if len(s) != len(t):
        return False
    
    freq = [0] * 26
    
    for i in range(len(s)):
        sc = ord(s[i]) - ord('a')
        freq[sc] += 1
        tc = ord(t[i]) - ord('a')
        freq[tc] -= 1

    for i in freq:
        if i != 0:
            return False

    return True
