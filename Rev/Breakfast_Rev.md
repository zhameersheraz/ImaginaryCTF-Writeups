# Breakfast Rev — ImaginaryCTF Writeup

**Challenge:** Breakfast Rev  
**Category:** Rev  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{BAABBAABBBABAAABAABAAAAABAAAAAAAABAABBBAABBABAAABAABAAAABBBBAABBBAABAABAAABAABAAAAABBAABABABABBAAAAAAABBAABBAAABAAAAABBAAABBBBAABBAAAABAABAAABBBAABBABAABAAABBBAAABABBAABBAABBBAABAAABABBABBBAABBABAABBAAABAABAABABAABBBABBAAABAABABABAABAAAABBBAAAAAAAABBABAAAABBABABAAAAAABABAABBAABABAABBBABAAABAABABAABBABBBABAAABBBAAA}`

---

## Background Knowledge (Read This First!)

### What is Rabin-Karp?

**Rabin-Karp** is a string search algorithm that uses hashing. It computes a rolling hash of substrings and compares hashes — much faster than comparing characters directly.

### What is the Encoding?

The program implements a "very silly" version of Rabin-Karp. The flag is encoded as a long binary string of `A`s and `B`s (essentially binary with `A=0, B=1`).

### How Do You Reverse It?

To recover the original flag from the Rabin-Karp hash, use the **Chinese Remainder Theorem**. The hash is computed modulo several values, and CRT reconstructs the original string from these residues.

*(The full source and solve are provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download and read the source

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O breakfast_rev.py
└─$ cat breakfast_rev.py
# → Rabin-Karp hash of binary A/B string
```

### Step 2 — Download and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <solve url> -O solve.py
└─$ python3 solve.py
# → Long binary string of A/B characters
# → ictf{BAABBAABBB...}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + CRT | Reverse the Rabin-Karp hash | ⭐⭐ Medium |

---

## Key Takeaways

- Rabin-Karp hashing is reversible via CRT when the moduli are known
- "Never write code on an empty stomach" — the implementation is intentionally messy
- Binary A/B encoding: A=0, B=1 — a simple but unusual encoding scheme
