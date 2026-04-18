# FNV-1a — ImaginaryCTF Writeup

**Challenge:** FNV-1a  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{x0r_15_4lm057_3qu4l_70_4dd1710n}`

---

## Description

> I've think this hash is pretty secure... prove me wrong

**Attachment:** Large hash value + source code + solve script

---

## Background Knowledge (Read This First!)

### What is FNV-1a?

**FNV-1a (Fowler-Noll-Vo)** is a non-cryptographic hash function. For each byte of input:

```
hash = hash XOR byte
hash = hash * FNV_prime
```

It is fast and simple — but NOT designed for security.

### Why is "XOR is almost equal to addition"?

Over GF(2) (binary field), XOR **is** addition. Over the integers, XOR and addition differ only in their carry behavior. For the FNV-1a hash, this relationship means the hash function has a linear structure that can be exploited — the XOR operations create a system of linear equations solvable over GF(2).

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O fnv1a.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{x0r_15_4lm057_3qu4l_70_4dd1710n}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Linear algebra over GF(2) to invert FNV-1a | ⭐⭐⭐ Hard |

---

## Key Takeaways

- FNV-1a is a non-cryptographic hash — never use it for security purposes
- XOR over GF(2) is addition — this creates an exploitable linear structure
- The flag "xor is almost equal to addition" is the mathematical key insight
