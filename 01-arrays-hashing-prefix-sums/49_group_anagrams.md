## Group Anagrams

**Pattern:** HashMap — Canonical Representation / Grouping

### Recognition

Need to group multiple objects that are equivalent
under some transformation/property.

Find a canonical representation such that:

equivalent objects → identical key

Then use:

canonical key → list of matching objects

### Frequency Signature

For lowercase English letters, represent each string
using a 26-element frequency array.

"eat", "tea", "ate"

all produce the same character-frequency signature.

Use the immutable frequency tuple as the HashMap key.

### Approach

For every string:

1. Create `freq[26]`
2. Count each character
3. Convert frequency array to an immutable tuple
4. Use tuple as dictionary key
5. Append string to that key's group

### Invariant

Strings with identical frequency signatures contain
the same characters with the same frequencies and
therefore belong to the same anagram group.

### Complexity

Let:
n = number of strings
k = maximum/average string length

Time:  O(n * k)
Space: O(n * k)

Sorted-key alternative:
Time: O(n * k log k)

### Pitfalls

- Python lists cannot be dictionary keys
- Convert frequency list to `tuple`
- `[0] * 26` assumes lowercase English letters
- Create a fresh frequency array for every string
- Don't accidentally share one frequency array across strings

### Python Concepts

`ord(char) - ord('a')`
→ converts lowercase character to index 0..25

`defaultdict(list)`
→ automatically creates an empty list for unseen keys

`tuple(freq)`
→ immutable/hashable representation usable as dict key

### 10-Second Recall

Need to group equivalent objects
→ derive canonical signature
→ signature becomes HashMap key
→ key maps to list of matching objects.

### Python

```python
def groupAnagrams(strs: list[str]) -> list[list[str]]:
    anagrams = defaultdict(list)
    for anagram in strs:
        freq = [0] * 26
        for char in anagram.lower():
            freq[ord(char) - ord('a')] += 1
        key = tuple(freq)
        anagrams[key].append(anagram)
    return list(anagrams.values())
