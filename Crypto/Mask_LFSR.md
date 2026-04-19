# Mask LFSR — ImaginaryCTF Writeup

**Challenge:** Mask LFSR  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 90 pts  
**Flag:** `ictf{4n_Unkn0wn_lf5r_15_571ll_L1n34r}`

---

## Description

> Twice the masks means twice the security right?...

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What is a Masked LFSR?

A **Masked LFSR** applies a bitwise mask to the LFSR output — hiding some bits before revealing others. With two masks, only certain bit positions of the output are shown.

### Why is it Still Linear?

Despite masking, the LFSR output is still a **linear function** of the initial state. Even with two masks applied, the relationship between observed bits and the internal state remains linear — exploitable via:

- Berlekamp-Massey (if enough output is observed)
- Linear algebra over GF(2) to reconstruct the state

The flag says it: "An unknown LFSR is still linear."

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O mask_lfsr.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{4n_Unkn0wn_lf5r_15_571ll_L1n34r}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Linear algebra over GF(2) to recover LFSR state | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Masking LFSR output does NOT hide its linearity — linear algebra still recovers the state
- Two masks = more obscuration, but still exploitable with enough observed output
- "An unknown LFSR is still linear" — the flag is the key insight
