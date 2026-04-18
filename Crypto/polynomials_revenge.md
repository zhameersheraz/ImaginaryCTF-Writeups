# polynomials revenge — ImaginaryCTF Writeup

**Challenge:** polynomials revenge  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 95 pts  
**Flag:** `ictf{p-4dic5_ar3_s0_fun_I_th1nk_sm4r+_h4d_a_fi3ld_d4y}`

---

## Description

> Trivariate? More like "poly"variate.

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What is p-adic expansion?

The **p-adic expansion** of a number represents it in base `p`. For polynomials, evaluating at `p` and expanding gives a way to recover polynomial coefficients — as long as the coefficients are smaller than `p`.

### What is the Extension from `polynomials`?

The original `polynomials` challenge involved bivariate polynomials. This version extends to **trivariate** (three variables). The approach is similar but requires an extra step:

1. Recover `igcd(f2, f3, f4)` — the integer GCD of three polynomials. With high probability (~60% of the time), this equals 1 digit.
2. Since `gcd = f0 * f1`, you now have `f0 * f1`
3. Since coefficients of `f0` and `f1` are each `< sqrt(p)`, coefficients of `f0*f1 < p`
4. Recover coefficients via **p-adic expansion** of `f0(p) * f1(p)`
5. Use the coefficient relations to recover `q + r`
6. Solve simultaneous equations for `q` and `r` → decrypt

---

## Solution

### Step 1 — Download challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O polynomials_revenge.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{p-4dic5_ar3_s0_fun_I_th1nk_sm4r+_h4d_a_fi3ld_d4y}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Polynomial GCD + p-adic expansion + simultaneous equations | ⭐⭐⭐ Hard |

---

## Key Takeaways

- p-adic expansions let you recover polynomial coefficients when they are bounded by `p`
- Integer GCD of polynomial evaluations is a powerful tool for factoring unknown polynomials
- Simultaneous equations on polynomial coefficient relations unlock the full solution
- This is a direct extension of the `polynomials` challenge — always revisit simpler variants first
