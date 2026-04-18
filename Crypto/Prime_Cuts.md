# Prime Cuts — ImaginaryCTF Writeup

**Challenge:** Prime Cuts  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{m041_m4n_15_4_w45h3d_4u7h0r_wh0_0nly_m4k35_lf5r_ch4ll3n635}`

---

## Description

> You can keep the primes, I'll keep the rest...

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What is LFSR?

A **Linear Feedback Shift Register (LFSR)** is a circuit that generates a pseudo-random bit sequence using XOR feedback. It is simple but cryptographically weak — given enough output bits, you can fully recover the internal state using the Berlekamp-Massey algorithm.

### What is the "Prime Cuts" Trick?

The challenge keeps only the **non-prime** outputs of the LFSR and discards the prime-indexed ones. This is a form of decimation (taking every nth output) — which weakens but does not eliminate the LFSR's structure.

### What is the Attack?

With enough decimated outputs, you can still recover the LFSR state using variants of Berlekamp-Massey or by brute-forcing the decimation pattern.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O prime_cuts.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{m041_m4n_15_4_w45h3d_4u7h0r_wh0_0nly_m4k35_lf5r_ch4ll3n635}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | LFSR state recovery from decimated output | ⭐⭐⭐ Hard |

---

## Key Takeaways

- LFSRs are broken by Berlekamp-Massey given enough output bits
- Decimation (skipping prime-indexed outputs) weakens but does not fix LFSR security
- The flag roasts the author for "only making LFSR challenges" — a running joke!
