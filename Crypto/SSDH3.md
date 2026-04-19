# SSDH3 ver. 1.07 — ImaginaryCTF Writeup

**Challenge:** SSDH3 ver. 1.07  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{3l_P?y_K0ngr0o_0.571024}`

---

## Description

> ??? (x2)

**Connection:** `nc 155.248.210.243 42107`

---

## Background Knowledge (Read This First!)

### What is Paillier Cryptosystem?

**Paillier** is a homomorphic public-key cryptosystem. A key property: given a ciphertext `E(m)`, you can compute `E(m + k)` without knowing `m`. This homomorphic property is the basis of many attacks on weakly implemented variants.

### What is Okamoto-Uchiyama?

**Okamoto-Uchiyama** is another probabilistic public-key scheme. Like Paillier, it has homomorphic properties that allow recovering plaintexts modulo a prime factor.

### What is the Attack?

Using ideas from both Paillier and Okamoto-Uchiyama:
1. Recover `A mod p` from the server interaction
2. Run the attack multiple times with different primes
3. Apply **CRT (Chinese Remainder Theorem)** to reconstruct the full `A`
4. Use `A` to decrypt the flag

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url>
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{3l_P?y_K0ngr0o_0.571024}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + pwntools | Interact with server, recover A mod p | ⭐⭐⭐ Hard |
| CRT | Combine multiple residues to recover full A | ⭐⭐ Medium |

---

## Key Takeaways

- Paillier and Okamoto-Uchiyama homomorphic properties enable recovering plaintexts mod p
- Running multiple times + CRT is a classic technique for recovering full values
- "???" (x2) = the challenge is doubly mysterious — you need to figure out the scheme from scratch
