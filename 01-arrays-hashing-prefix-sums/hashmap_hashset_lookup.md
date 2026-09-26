# Hash Map / Hash Set Lookup

## Recognition

Think hashing when I repeatedly need:

- "Have I seen X?"
- "Does X exist?"
- "How many X?"
- "Where did X occur?"
- "Can I find the required complement?"

Especially when brute force searches the array for every element.

## Core Idea

Trade memory for faster lookup.

Repeated O(n) search
→ store previous information
→ O(1) average lookup

Often converts:

O(n²) → O(n)

## HashSet

Use when only existence matters.

value → exists

## HashMap

Use when additional information matters.

Examples:

value → index
value → frequency
value → last index
prefix sum → frequency

## Key Invariant

Before processing the current element, the hash structure
contains information about previously processed elements.

## Complement Pattern

If:

a + b = target

rewrite as:

b = target - a

For each `a`:

1. Compute required `b`
2. Check whether `b` was previously seen
3. Store `a`

## Common Pitfalls

- Inserting before lookup when distinct indices matter
- Using Map when Set is sufficient
- Forgetting duplicates
- Forgetting O(n) extra space
- Assuming hash operations are guaranteed O(1)
- Ignoring integer overflow
- Using hashing when ordering is important

## Complexity

Typical:

Time:  O(n) average
Space: O(n)
