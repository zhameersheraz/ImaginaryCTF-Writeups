# bitD2cision — ImaginaryCTF Writeup

**Challenge:** bitD2cision  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 80 pts  
**Flag:** `ictf{chEck_t0rsion_aft3r_pa1ring_3f1d7aec}`

---

## Description

> Make your decision bit by bit, again!

**Attachment:** `output.txt` with `n` and `output` values

---

## Background Knowledge (Read This First!)

### What is a Torsion Check?

In elliptic curve cryptography, **torsion points** are points of finite order. Checking if a point has a specific order in a particular subgroup is called a torsion check.

### What is the Pairing?

**Pairings** (like Weil or Tate pairings) are bilinear maps from elliptic curve points to a finite field. They are used in advanced cryptography (BLS signatures, zk-SNARKs, etc.).

### What is the Decode Process?

Looking at the solve script:

```python
p = 0x1a0111ea397fe69a4b1ba7b6434bacd764774b84f38512bf6730d2a0f6b0f6241eabfffeb153ffffb9feffffffffaaab
# This is the BLS12-381 field prime

for out in output:
    if pow(out, 52437899, p) == 1:
        res += "1"
    else:
        res += "0"
```

Each output value is checked: if `out^52437899 ≡ 1 (mod p)`, the bit is 1; otherwise 0. This is checking **membership in a specific subgroup** — a torsion check after a pairing operation!

---

## Solution

### Step 1 — Download the output file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O output.txt
└─$ cat output.txt
```

### Step 2 — Create and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from Crypto.Util.number import long_to_bytes
import ast

with open("output.txt") as f:
    n      = ast.literal_eval(f.readline())
    output = ast.literal_eval(f.readline())

p   = 0x1a0111ea397fe69a4b1ba7b6434bacd764774b84f38512bf6730d2a0f6b0f6241eabfffeb153ffffb9feffffffffaaab
res = ""

for out in output:
    if pow(out, 52437899, p) == 1:
        res += "1"
    else:
        res += "0"

print(long_to_bytes(int(res, 2)))
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{chEck_t0rsion_aft3r_pa1ring_3f1d7aec}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Torsion subgroup check via modular exponentiation | ⭐⭐ Medium |

---

## Key Takeaways

- `pow(x, k, p) == 1` checks if `x` is in the subgroup of order dividing `k` — a torsion check
- The BLS12-381 prime is recognizable — knowing common elliptic curve parameters helps
- After pairing, torsion checks encode bits — an unusual but elegant encoding scheme
