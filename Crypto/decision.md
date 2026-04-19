# decision — ImaginaryCTF Writeup

**Challenge:** decision  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 75 pts  
**Flag:** `ictf{dec1d3_my_fl4g_4nd_m9_b1ts_by_LLL_3ac4ed91}`

---

## Description

> I believe you can distinguish between primes and random numbers...

**Attachment:** `output.txt` with primes and ciphertext

---

## Background Knowledge (Read This First!)

### What is the Problem?

Given a list of large primes and a list of output values, determine whether each output value is related to those primes or is random. This is a **decision problem** — prime vs not prime in some algebraic structure.

### What is LLL?

**LLL (Lenstra-Lenstra-Lovász)** is a lattice basis reduction algorithm. It finds short vectors in a lattice, which can be used to solve many problems in cryptography — including this one.

### What is BKZ?

**BKZ (Block Korkine-Zolotarev)** is a stronger lattice reduction algorithm than LLL. It finds even shorter vectors at the cost of more computation.

### How Does the Solve Work?

For each output value `i`, a lattice matrix `M` is constructed where:
- The diagonal contains 1s (for identity)
- The last column contains the primes
- The bottom-right entry is the current output value `i`

After BKZ reduction, if the first row of the reduced matrix contains only 0s and -1s, the value `i` was generated from the primes (bit = 1). Otherwise, bit = 0.

---

## Solution

### Step 1 — Download the output file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O output.txt
```

### Step 2 — Create and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

```python
from Crypto.Util.number import *
import ast
from sage.all import *
from tqdm import tqdm

with open("output.txt") as f:
    Primes = ast.literal_eval(f.readline())
    out    = ast.literal_eval(f.readline())

res = ""
M = Matrix(ZZ, 65, 65)
for j in range(64):
    M[j, j]  = 1
    M[j, -1] = Primes[j]

for i in tqdm(out, total=len(out)):
    M[64, -1] = i
    L = M.BKZ()
    if all(int(c) == 0 or int(c) == -1 for c in L[0]):
        res += "1"
    else:
        res += "0"

print(long_to_bytes(int(res, 2)))
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → ictf{dec1d3_my_fl4g_4nd_m9_b1ts_by_LLL_3ac4ed91}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath (BKZ) | Lattice reduction to decide bit value per output | ⭐⭐⭐ Hard |

---

## Key Takeaways

- BKZ lattice reduction solves the decision problem: is this value prime-related or random?
- The lattice structure encodes the relationship between primes and the output value
- Short vectors in the reduced lattice indicate prime-relatedness — a 0/-1 pattern
