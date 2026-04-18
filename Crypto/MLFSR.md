# MLFSR — ImaginaryCTF Writeup

**Challenge:** MLFSR  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 150 pts  
**Flag:** `ictf{lfsr_is_approximately_equal_to_lcg_right?}`

---

## Description

> Following the steps of @moai_man, I made another masked linear feedback shift register challenge! Except the register is a bit bigger than what you might expect 😛

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What is a Masked LFSR?

A **Masked LFSR (MLFSR)** applies a mask (bitwise AND or similar) to the LFSR output before revealing it. This truncates the output, making standard Berlekamp-Massey attacks harder — hence "masked."

### What is the Connection to LCG?

The flag says "LFSR is approximately equal to LCG." A **Linear Congruential Generator (LCG)** produces: `x_{n+1} = a*x_n + c mod m`. An LFSR over GF(2^n) has very similar structure. The truncated output of an MLFSR behaves like a **Truncated LCG**.

### What is Stern's Attack on LCG?

**Stern's attack** recovers LCG parameters from truncated outputs using lattice reduction (LLL). Since the MLFSR behaves like a truncated LCG, you adapt Stern's attack:

1. Model the MLFSR as a truncated LCG
2. Apply LLL lattice reduction to the truncated outputs
3. Recover the full internal state

*(The full solve script with explanations is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O mlfsr.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{lfsr_is_approximately_equal_to_lcg_right?}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Stern's LCG attack adapted for MLFSR + LLL lattice reduction | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Truncated MLFSR output is equivalent to truncated LCG — same attacks apply
- Stern's attack uses LLL to recover LCG state from truncated outputs
- IACR 2022/1134 is an alternative approach worth reading
- Always look for mathematical equivalences between different PRNG structures
