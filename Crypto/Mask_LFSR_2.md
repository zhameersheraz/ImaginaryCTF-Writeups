# Mask LFSR 2 — ImaginaryCTF Writeup

**Challenge:** Mask LFSR 2  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{d1v150r5_0f_7h3_m1n1m4l_p0lyn0m14l}`

---

## Description

> Twice the masks Twice the ciphers Twice the plaintext… actually scratch that last part

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What Changed from Mask LFSR 1?

More complexity is added — two ciphers, two masks. But the core weakness remains: **LFSRs are linear**.

### What are Divisors of the Minimal Polynomial?

An LFSR's behavior is completely characterized by its **minimal polynomial** — the shortest polynomial over GF(2) that generates the observed sequence. The **divisors** of this polynomial correspond to the individual LFSR components.

The flag says it: "divisors of the minimal polynomial" — this is the key to factoring the combined LFSR structure and recovering the state.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O mask_lfsr2.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{d1v150r5_0f_7h3_m1n1m4l_p0lyn0m14l}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Berlekamp-Massey + minimal polynomial factoring | ⭐⭐⭐ Hard |

---

## Key Takeaways

- The minimal polynomial of a combined LFSR factors into its component polynomials
- Divisors of the minimal polynomial reveal the individual LFSR structures
- Doubling the complexity of a linear system does not make it non-linear
