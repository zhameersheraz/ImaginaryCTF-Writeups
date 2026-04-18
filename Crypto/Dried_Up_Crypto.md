# Dried Up Crypto — ImaginaryCTF Writeup

**Challenge:** Dried Up Crypto  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{4_p4r714l_1nf0rm4710n_r54_w17h0u7_l4771c35}`

---

## Background Knowledge (Read This First!)

### What is Partial Information RSA?

In standard RSA, you know `n`, `e`, and `c`. In **partial information RSA**, you are also given some bits of `p` or `q` — but not all. Recovering the full prime from partial information is the challenge.

### What is Branch and Prune?

**Branch and Prune** is an algorithm for recovering a prime `p` from partial known bits. It works like a tree search:

1. Start from the known bits
2. At each unknown bit position, **branch** — try both 0 and 1
3. **Prune** branches that are inconsistent with the known partial information
4. Continue until you find the full prime

This is efficient when enough bits are known — typically around 50% of the bits of `p`.

---

## Solution

### Step 1 — Download the challenge

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O dried_up_crypto.py
└─$ cat dried_up_crypto.py
# → RSA with partial bits of p provided
```

### Step 2 — Use the branch and prune implementation

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget https://raw.githubusercontent.com/jvdsn/crypto-attacks/master/attacks/factorization/branch_and_prune.py
└─$ nano solve.py
```

```python
from branch_and_prune import factorize
from Crypto.Util.number import long_to_bytes

# Paste n, e, c, and partial bits of p from the challenge
n = ...
e = ...
c = ...
partial_p = ...  # known bits of p

p, q = factorize(n, partial_p)
d = pow(e, -1, (p-1)*(q-1))
print(long_to_bytes(pow(c, d, n)))
# → ictf{4_p4r714l_1nf0rm4710n_r54_w17h0u7_l4771c35}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{4_p4r714l_1nf0rm4710n_r54_w17h0u7_l4771c35}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Branch and Prune (jvdsn) | Recover full prime from partial bits | ⭐⭐⭐ Hard |
| Python 3 | RSA decryption | ⭐ Easy |

---

## Key Takeaways

- Partial information RSA is solvable without lattices using branch and prune
- The jvdsn crypto-attacks repository is an essential resource for CTF crypto
- Branch and prune is efficient when ~50% or more of the prime bits are known
