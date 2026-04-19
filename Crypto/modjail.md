# modjail — ImaginaryCTF Writeup

**Challenge:** modjail  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{p1Ck3d_y0Ur_W@y_pa$7_th3_P1cKy_m0du1u5}`

---

## Description

> Submit anything... that meets my modular condition!

**Connection:** `nc 155.248.210.243 42114`

---

## Background Knowledge (Read This First!)

### What is a Modular Condition?

The server checks that your submitted value satisfies some condition modulo a specific number. For example: `x mod m == target`. Finding such an `x` is easy with modular arithmetic — just solve for `x` directly.

### What Makes This Hard?

The modulus `m` is **picky** — it may have special structure (smooth, prime, etc.) that constrains which values work. The solve script finds the right approach based on the modulus structure.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Connect and observe the condition

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42114
# → Read the modular condition
```

### Step 2 — Download and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <solve url> -O solve.py
└─$ python3 solve.py
# → ictf{p1Ck3d_y0Ur_W@y_pa$7_th3_P1cKy_m0du1u5}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + pwntools | Compute and submit the correct modular value | ⭐⭐ Medium |

---

## Key Takeaways

- Modular arithmetic problems are always solvable if you understand the structure
- "Picky modulus" means the modulus has special properties constraining valid inputs
- modjail is the first in a series — each sequel adds more conditions
