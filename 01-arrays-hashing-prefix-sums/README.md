# DSA Patterns - Arrays, Hashing & Prefix Sums

This folder is a fast-glance reference for recurring interview patterns in arrays, hashing, and prefix sums.

## How to use this folder

1. Learn the **recognition signal** for each pattern.
2. Memorize the **core invariant** rather than a single implementation.
3. Practice representative problems until the pattern becomes automatic.
4. Record every solved problem in `problems.md`, especially mistakes and missed edge cases.

## Pattern index

| Pattern | Core idea | Typical time | Typical space |
|---|---|---:|---:|
| Hash Set / Hash Map Lookup | Trade memory for O(1) average lookup | O(n) | O(n) |
| Frequency Counting | Count occurrences to compare, group, or validate | O(n) | O(k) |
| Two Sum / Complement Lookup | Store seen values and query the needed complement | O(n) | O(n) |
| Prefix Sum | Convert repeated range sums into O(1) queries | O(n) build | O(n) |
| Prefix Sum + Hash Map | Count/locate subarrays satisfying a sum relation | O(n) | O(n) |
| Running Prefix/Suffix State | Precompute information from the left/right | O(n) | O(n) or O(1) |
| Difference Array | Apply many range updates efficiently | O(n + q) | O(n) |
| In-place Array Marking | Reuse array positions as state when values have constrained range | O(n) | O(1) |

## Interview recognition checklist

Ask:

- Do I need fast membership or frequency lookup? -> **Hashing**
- Do I need a range sum repeatedly? -> **Prefix sum**
- Do I need the number of subarrays with a target sum? -> **Prefix sum + hash map**
- Can the answer be expressed as `current prefix - earlier prefix = target`? -> **Prefix sum + hash map**
- Are there many operations over intervals `[l, r]`? -> **Difference array**
- Is the value range tightly bounded and does the problem allow modifying the input? -> **In-place marking**
