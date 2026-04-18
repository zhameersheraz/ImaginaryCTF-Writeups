# univariate — ImaginaryCTF Writeup

**Challenge:** univariate  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{p-1_g0es_aB$olU7eLy_w1lD!!!}`

---

## Description

> I love univariate polynomials...

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What is a Univariate Polynomial?

A **univariate polynomial** has only one variable: `f(x) = a_n*x^n + ... + a_1*x + a_0`. In cryptography, polynomials over finite fields are used in many constructions.

### What is Pollard's p-1 Attack?

**Pollard's p-1** is a factoring algorithm that works when `p-1` (one less than a prime factor of `n`) is **smooth** — meaning all its prime factors are small.

The flag `p-1_g0es_aB$olU7eLy_w1lD` is a direct hint — the `p-1` of some prime in the challenge is smooth, enabling this attack.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O univariate.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{p-1_g0es_aB$olU7eLy_w1lD!!!}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Pollard's p-1 factoring + polynomial operations | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Pollard's p-1 factors `n` when `p-1` is smooth — always check for smooth `p-1`
- The flag literally encodes the attack: "p-1 goes absolutely wild"
- Univariate polynomials over finite fields are a building block for many crypto schemes
