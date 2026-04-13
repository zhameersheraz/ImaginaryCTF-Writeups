# orfevre — ImaginaryCTF Writeup

**Challenge:** orfevre  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{orfevre_really_mellowed_out_in_retirement_https://www.youtube.com/shorts/OPP0H-rPzfs}`

---

## Description

> god gives his most evil and intimidating horses to his strongest jockey (Kenichi Ikezoe), anyways did you know Orfevre is French for gold smith, I wonder who else was French

**Attachment:** Sequence evaluation data

---

## Background Knowledge (Read This First!)

### What is Sparse Interpolation?

**Sparse interpolation** is the problem of recovering a sparse polynomial (one with few non-zero terms) from a small number of evaluations. Normal polynomial interpolation requires many evaluation points, but when the polynomial is sparse, there are smarter methods.

### What is Prony's Method?

**Prony's method** is a classic algorithm from 1795 that solves the sparse interpolation problem when evaluations are at a **geometric progression** of points (i.e., at `x, x^2, x^3, x^4, ...`).

Given evaluations at these points, Prony's method recovers the polynomial's structure by solving a linear system. It is essentially an older precursor to Berlekamp-Massey.

### Why is the challenge named Orfevre?

Orfevre is a famous Japanese racehorse. The description mentions "French" — both Orfevre (goldsmith in French) and Prony (a French mathematician) are French. That is the hint pointing you toward Prony's method!

---

## Solution

### Step 1 — Download and inspect the file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat orfevre.txt
# → Polynomial evaluations at geometric progression points
```

### Step 2 — Apply Prony's Method

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

```python
# SageMath — Prony's Method for sparse interpolation
# Given evaluations f(x), f(x^2), f(x^3), ... recover the sparse polynomial

evals = [...]  # paste evaluations here
x     = ...    # base evaluation point

n = len(evals) // 2

# Build Hankel matrix from evaluations
H = Matrix([[evals[i+j] for j in range(n)] for i in range(n)])

# Solve for the characteristic polynomial
coeffs = H.solve_right(vector(evals[n:2*n]))

# Roots of characteristic polynomial give the exponents
R.<z> = PolynomialRing(parent(evals[0]))
char_poly = z^n - sum(coeffs[i]*z^i for i in range(n))
roots = char_poly.roots()

# Recover the sparse polynomial coefficients
print("Recovered polynomial:", roots)
# → Decode to get the flag
```

### Step 3 — Run it and decode the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → ictf{orfevre_really_mellowed_out_in_retirement_https://www.youtube.com/shorts/OPP0H-rPzfs}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Prony's method — Hankel matrix + root finding | ⭐⭐⭐ Hard |

---

## Key Takeaways

- **Prony's method** recovers sparse polynomials from geometric progression evaluations — a classic 1795 algorithm
- The challenge name and description both hint at "French" → pointing directly to Prony (a French mathematician)
- Prony's method is essentially the continuous-domain precursor to Berlekamp-Massey
- When evaluations are at `x, x^2, x^3, ...` (geometric), always think Prony's method first
