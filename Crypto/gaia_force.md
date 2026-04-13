# gaia force — ImaginaryCTF Writeup

**Challenge:** gaia force  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{weirdo_horse_that_has_silvers_in_both_turf_and_dirt_g1s}`

---

## Description

> Yesterday was the mile championship, sadly Gaia Force placed 2nd, meaning he likely will not achieve a single g1 win before his retirement 😦

**Attachment:** Oracle + matrix data

---

## Background Knowledge (Read This First!)

### What is a Matrix with Eigenvalues?

A **matrix** `M` can be **diagonalized** if it has distinct eigenvalues `c1` and `c2`. Diagonalization means:

```
M = P * D * P^(-1)
```

where `D` is a diagonal matrix with eigenvalues on the diagonal. This makes computing powers of `M` very easy.

### What is a Vandermonde Representation?

The **Vandermonde matrix** is built from powers of values. Combined with diagonalization, it lets you express matrix sequences as polynomial evaluations at the eigenvalues `c1` and `c2`.

### What is the Attack?

The challenge has an oracle that outputs bits. The key insight:

- `M` has (almost certainly unique) eigenvalues `c1` and `c2`
- This lets you set up a polynomial: `f = a2*x^(n+1) + a1*x^n - b2*x - b1`
- This polynomial contains `c1` and `c2` as roots
- **For a random polynomial, the probability it has a root is about `1 - 1/e` ≈ 63%**
- If `b1` and `b2` are random (bit = `0`), sometimes `f` has a root
- If `b1` and `b2` are derived from the eigenvalues (bit = `1`), `f` will almost always have a root
- Use this probability difference across 64 samples to distinguish `0` from `1`

---

## Solution

### Step 1 — Download and inspect the files

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat gaia_force.txt
# → Matrix M, oracle outputs, ciphertext
```

### Step 2 — Diagonalize M and find eigenvalues

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

```python
# SageMath
M  = Matrix(...)   # paste M here
c1, c2 = M.eigenvalues()

# For each oracle sample, set up the polynomial f
# f = a2*x^(n+1) + a1*x^n - b2*x - b1
# Check if f has a root — if YES across all 64 samples → bit is 1

recovered_bits = []
for sample in oracle_samples:
    a1, a2, b1, b2, n = sample
    R.<x> = PolynomialRing(base_field)
    f = a2*x^(n+1) + a1*x^n - b2*x - b1
    has_root = len(f.roots()) > 0
    recovered_bits.append(1 if has_root else 0)

# Convert bits to flag
flag = int(''.join(map(str, recovered_bits)), 2).to_bytes(...)
print(flag.decode())
# → ictf{weirdo_horse_that_has_silvers_in_both_turf_and_dirt_g1s}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → ictf{weirdo_horse_that_has_silvers_in_both_turf_and_dirt_g1s}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Matrix diagonalization, eigenvalues, polynomial root finding | ⭐⭐⭐ Hard |

---

## Key Takeaways

- **Matrix diagonalization** via eigenvalues makes computing matrix sequences tractable
- **Vandermonde representation** lets you express matrix sequences as polynomial evaluations
- The probability that a random polynomial has a root (~63%) is used as a statistical oracle to distinguish bits
- Across 64 samples, the probability of a false positive becomes astronomically small
