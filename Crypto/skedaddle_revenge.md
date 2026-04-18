# skedaddle revenge — ImaginaryCTF Writeup

**Challenge:** skedaddle revenge  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 150 pts  
**Flag:** `ictf{d1d_y0u_u53_l4tt1c3_3num3r4t10n_0r_s0m3th1ng_else?}`

---

## Description

> surely 128 bits is safe, right?

**Connection:** `nc 155.248.210.243 42168`

---

## Background Knowledge (Read This First!)

### What Changed from skedaddle?

The original skedaddle used 64-bit values with a 33-bit brute force window. This version upgrades to **128 bits** — making naive brute force completely infeasible (`2^64` operations minimum).

### What is the Mathematical Reduction?

The solution reduces the problem to:

```
A * x_hi + B * x_lo = 2 * (x_hi & x_lo)
```

Where `x_hi` and `x_lo` are the upper and lower halves of the 128-bit value `x`.

### What is Hensel Lifting?

**Hensel's lemma** lets you "lift" a solution modulo a small number to a solution modulo a larger power of that number. It is like Newton's method but for modular arithmetic. Starting from a small solution, you iteratively refine it to get the full solution.

### What is Fast Lattice Enumeration?

After Hensel lifting gives partial candidates, **lattice enumeration** (using algorithms like BKZ or LLL) efficiently searches the remaining solution space. A brute force approach takes ~1 hour on a decent CPU — lattice enumeration is much faster.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O skedaddle_revenge.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# Known solution: x = 0x77140a2f515f7d36838035cbd1a4412c
# → ictf{d1d_y0u_u53_l4tt1c3_3num3r4t10n_0r_s0m3th1ng_else?}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Hensel lifting + lattice enumeration | ⭐⭐⭐ Hard |
| `nc` | Connect to the challenge server | ⭐ Easy |

---

## Key Takeaways

- 128 bits eliminates brute force — you need a mathematical reduction
- The problem reduces to a bilinear equation `A*x_hi + B*x_lo = 2*(x_hi & x_lo)`
- Hensel lifting + lattice enumeration solves it efficiently
- The flag asks if you used lattice enumeration or something else — multiple approaches exist!
