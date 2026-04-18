# Complex Curve Crypto — ImaginaryCTF Writeup

**Challenge:** Complex Curve Crypto  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 200 pts  
**Flag:** `ictf{it's all lattices...?}`

---

## Description

> Wait, where are the elliptic curves?

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What is a j-invariant?

The **j-invariant** is a number that uniquely identifies an elliptic curve up to isomorphism over an algebraically closed field. Two curves have the same j-invariant if and only if they are isomorphic.

### What is a Complex Lattice?

Every elliptic curve over ℂ corresponds to a **complex lattice** — a discrete subgroup of ℂ. The j-invariant of the curve can be computed from this lattice. Going between curves via isogenies corresponds to transforming the lattice.

### What is a Möbius Transformation?

A **Möbius transformation** (linear fractional transformation) is a map of the form:

```
f(z) = (az + b) / (cz + d)
```

In this challenge, the relationship between the input and output j-invariants corresponds to a Möbius transformation that can be recovered using LLL lattice reduction.

### What is the Attack?

1. Compute the complex lattices corresponding to the input and output j-invariants
2. Use **LLL** to recover the Möbius transformation matrix between them
3. Factor this matrix into its constituent isogenies and a post-isomorphism
4. Retrace the path to recover the digits of the flag

*(A full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O complex_curve.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{it's all lattices...?}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Complex lattice computation + LLL + isogeny factoring | ⭐⭐⭐ Hard |

---

## Key Takeaways

- j-invariants encode the isomorphism class of an elliptic curve
- The transformation between j-invariants corresponds to a Möbius transformation recoverable via LLL
- "It's all lattices" — almost every hard crypto challenge reduces to a lattice problem
- A blog post explanation was planned by the author — worth following up!
