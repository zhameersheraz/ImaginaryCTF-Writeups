# polynomials — ImaginaryCTF Writeup

**Challenge:** polynomials  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{l1ne4r_di0phan+1n3_equat1on!}`

---

## Background Knowledge (Read This First!)

### What is a Linear Diophantine Equation?

A **Linear Diophantine equation** has the form `ax + by = c`. It has integer solutions if and only if `c` is a multiple of `gcd(a, b)`. This is Bezout's identity.

### What is the Key Insight?

For two moduli `n1` and `n2`:

```
a % n1 - a % n2 = (a - k*n1) - (a - k*n2) = k*(n2 - n1)
```

Therefore: `(a % n1) - (a % n2)` divides `gcd(n1, n2)`.

By choosing two polynomials whose polynomial GCD divides `x - 1`, you can factor `n` using **Pollard's p-1 factorization**, then decrypt normally.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O polynomials.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{l1ne4r_di0phan+1n3_equat1on!}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Polynomial GCD + Pollard's p-1 factorization | ⭐⭐⭐ Hard |

---

## Key Takeaways

- The difference of two modular reductions is divisible by the GCD of the moduli
- Polynomial GCD over a ring can reveal factors of the RSA modulus `n`
- Pollard's p-1 factorization works when `p-1` is smooth — here the polynomial structure creates this condition
