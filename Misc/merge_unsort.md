# merge unsort — ImaginaryCTF Writeup

**Challenge:** merge unsort  
**Category:** Misc  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{is_n=8_possible_though?}`

---

## Description

> merge sort but I'm dumb and don't know how to compare numbers, doesn't work most the time but makes a good shuffler... right?

**Attachment:** Source code + solve script

---

## Background Knowledge (Read This First!)

### What is Merge Sort?

**Merge sort** is a divide-and-conquer sorting algorithm that splits an array in half, sorts each half, then merges them. When the comparison function is broken (random), it becomes a shuffler instead.

### What is a Uniform Shuffle?

A **uniform shuffle** means every permutation of the array is equally likely. The classic Fisher-Yates shuffle achieves this. A broken merge sort does NOT — it has statistical biases.

### What is the Vulnerability?

When `n` is NOT a power of 2, the broken merge sort does not produce a uniform distribution over all permutations. Some permutations are more likely than others. By observing enough shuffles, you can statistically distinguish this shuffler from a truly random one — and use that to recover information.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download and inspect the source

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat merge_unsort.py
# → Broken merge sort used as a shuffler
# → Key is embedded in the shuffle output
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{is_n=8_possible_though?}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Statistical analysis of shuffle distribution | ⭐⭐⭐ Hard |

---

## Key Takeaways

- A non-uniform shuffle leaks statistical information — enough to distinguish it from random
- Non-power-of-2 array sizes break merge sort's uniformity
- The flag question "is n=8 possible though?" hints at the power-of-2 boundary condition
