# Devious Hash — ImaginaryCTF Writeup

**Challenge:** Devious Hash  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 85 pts  
**Flag:** `ictf{devious_mersenne_prime_makes_root_finding_easy_actually}`

---

## Description

> modern cryptographic hashes are efficient and secure... my hash is neither 😃
> Note: the flag is left padded, so check if output **contains** `ictf{` instead of starts with.

**Attachment:** Custom hash implementation

---

## Background Knowledge (Read This First!)

### What is a Mersenne Prime?

A **Mersenne prime** is a prime of the form `2^p - 1`. Examples: 3, 7, 31, 127, 8191...

Mersenne primes have special mathematical properties. In particular, finding roots of polynomials modulo a Mersenne prime is much easier than for a general prime — there are efficient algorithms that exploit the structure of `2^p - 1`.

### What is the Vulnerability?

The custom hash uses a Mersenne prime as its modulus. This makes the hash function invertible — you can find a preimage (input that produces a given hash output) by solving the resulting polynomial equation, which is easy because of the Mersenne prime structure.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O devious_hash.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → Search for ictf{ in output (it may be left-padded)
# → ictf{devious_mersenne_prime_makes_root_finding_easy_actually}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Polynomial root finding over Mersenne prime field | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Mersenne primes `(2^p - 1)` make polynomial root finding much easier than general primes
- Custom hash functions are almost always broken — never roll your own crypto
- Left-padded flags mean you need `contains` not `startswith` when checking output
