# bivariate — ImaginaryCTF Writeup

**Challenge:** bivariate  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 110 pts  
**Flag:** `ictf{symmetry_of_bivariate_polynomials_is_zany}`

---

## Description

> I love bivariate polynomials...

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What is a Bivariate Polynomial?

A **bivariate polynomial** has two variables: `f(x, y)`. These appear in multivariate cryptography and are harder to analyze than univariate polynomials.

### What is the Symmetry Property?

The flag says "symmetry of bivariate polynomials is zany." For symmetric bivariate polynomials, `f(x, y) = f(y, x)`. This symmetry creates additional algebraic relations that can be exploited.

### What is the Attack?

The symmetry of `f(x, y)` means you can set up additional equations by swapping `x` and `y`. Combined with the given evaluations, this creates an overdetermined system solvable via resultants or Gröbner bases.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O bivariate.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{symmetry_of_bivariate_polynomials_is_zany}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Symmetric polynomial system + Gröbner basis / resultants | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Symmetric bivariate polynomials `f(x,y) = f(y,x)` provide extra equations for free
- Gröbner bases solve overdetermined polynomial systems efficiently
- The sequel to `univariate` — each challenge adds one more variable and dimension of difficulty
