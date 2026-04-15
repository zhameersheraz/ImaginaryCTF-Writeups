# Symboli Rudolph 2 — ImaginaryCTF Writeup

**Challenge:** Symboli Rudolph 2  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 125 pts  
**Flag:** `ictf{I_stroked_Rudolphs_Body_his_coat_as_gentle_as_touching_the_finest_velvet}`

---

## Description

> Rudolph won 7 g1 races during his career, a record that was unbroken for nearly 40 years among JRA horses, in accordance with this achievement we've doubled the difficulty of the previous challenge

**Attachment:** Harder Merkle-Hellman variant

---

## Background Knowledge (Read This First!)

### What Changed from Symboli Rudolph 1?

The easy ratio attack no longer works. This version requires proper **linear algebra over GF(2^N)**.

### What is GF(2^N)?

**GF(2^N)** is a finite field (Galois Field) where elements are polynomials with binary coefficients. Multiplication in this field is polynomial multiplication modulo an irreducible polynomial.

### What is the Key Insight?

Multiplication by a constant in GF(2^N) is a **linear map** over the coefficient vector. This means:

- Each element of the public key is a linear transformation of the private key
- Together they form an **overdetermined linear system over GF(2)**
- Solve the system → recover the private key → decrypt the flag

---

## Solution

### Step 1 — Download and read the source

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat rudolph2.txt
# → Public key, ciphertext, GF parameters
```

### Step 2 — Set up and solve the linear system

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

```python
# SageMath
N   = ...    # GF(2^N) degree
pub = [...]  # public key elements

# Each public key element = linear map applied to private key
# Build the coefficient matrix over GF(2)
rows = []
for r in pub:
    # Multiplication by r as a linear map: build N×N matrix over GF(2)
    row = []
    for i in range(N):
        e_i = 2^i
        row.append(vector(GF(2), [(r * e_i >> j) & 1 for j in range(N)]))
    rows.append(row)

# Stack into overdetermined system and solve
M = Matrix(GF(2), rows)
b = vector(GF(2), ciphertext_bits)
solution = M.solve_right(b)

# Convert solution to flag
from Crypto.Util.number import long_to_bytes
flag_int = int(''.join(map(str, solution)), 2)
print(long_to_bytes(flag_int))
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → ictf{I_stroked_Rudolphs_Body_his_coat_as_gentle_as_touching_the_finest_velvet}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Linear algebra over GF(2^N) | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Multiplication in GF(2^N) is a linear map — this makes the knapsack solvable via linear algebra
- An overdetermined system over GF(2) is solved efficiently with Gaussian elimination
- "Doubled the difficulty" = need proper field theory instead of the simple ratio trick
