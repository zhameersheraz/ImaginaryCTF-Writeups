# xor thing — ImaginaryCTF Writeup

**Challenge:** xor thing  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 80 pts  
**Flag:** `ictf{XOR_m337_in_th3_m1ddl3}`

---

## Description

> i can't come up with flavortext just solve it

**Connection:** `nc 155.248.210.243 42121`

---

## Background Knowledge (Read This First!)

### What is Meet-in-the-Middle?

**Meet-in-the-Middle (MITM)** is an attack on ciphers that apply multiple operations sequentially. Instead of brute forcing all combinations from one end, you:

1. Compute all possible intermediate values going **forward** from the plaintext
2. Compute all possible intermediate values going **backward** from the ciphertext
3. Find a **match** in the middle

This reduces `2^(2n)` brute force to `2^n` forward + `2^n` backward = `2^(n+1)` total — exponentially faster.

### How is XOR related?

XOR operations are linear — making meet-in-the-middle particularly effective. The "meet in the middle" title of the flag confirms the intended approach.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O xor_thing.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{XOR_m337_in_th3_m1ddl3}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Meet-in-the-middle attack on XOR cipher | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Meet-in-the-Middle reduces `2^(2n)` brute force to `2^(n+1)` — massive speedup
- XOR's linearity makes it especially vulnerable to algebraic attacks
- The flag literally says "XOR meet in the middle" — always read the flag for hints about the method
