# Equinox — ImaginaryCTF Writeup

**Challenge:** Equinox  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 150 pts  
**Flag:** `ictf{omg_xor_is_just_like_multiplication_chat}`

---

## Description

> Elliptic curves with bit shenanigans what could go wrong... Please test your solutions locally before spamming the server.

**Connection:** `nc 155.248.210.243 42125`

---

## Background Knowledge (Read This First!)

### What is special about the curve order `E = 2^214 + 7`?

The curve group order is a prime of the special form `2^214 + 7`. Numbers like this have an unusual property involving **XOR and negation modulo E**.

For a random integer `x < E`, there is roughly a **1-in-4 chance** that:

```
x XOR (2^214 - 9)  ≡  -x  (mod E)
```

This means XOR-ing with the constant `(2^214 - 9)` behaves almost like negating in the group — a huge mathematical shortcut the challenge is built around.

### How does the challenge use hashing?

The server hashes strings and uses the hash values as scalars on the elliptic curve. The goal is to make two hashes combine in a specific way using XOR.

### What is the attack plan?

1. Generate a large pool of random strings
2. Compute `hash(string)` for each one
3. Use **linear algebra over GF(2)** (binary field) to find a *subset* of those strings whose hashes XOR together to equal `2^214 - 9`
4. Send that subset to the server — the XOR sum flips a sign on the elliptic curve, giving you the key

### Elliptic Curve Negation Trick

For any elliptic curve point `X` and scalar `a`:

```
(-a * X).y  =  -((a * X).y)  mod p
```

So if you force `a_2 ≡ -a_1 (mod E)`, then:

```
hint_1 + hint_2  =  2 * key  (mod p)
```

You can now solve for `key` directly!

---

## Solution

### Step 1 — Create the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

Paste this:

```python
import os, hashlib

E = 2**214 + 7
pool = []
for _ in range(300):
    s = os.urandom(16).hex()
    h = int(hashlib.sha256(s.encode()).hexdigest(), 16) % E
    pool.append((s, h))

# SageMath — linear algebra over GF(2) to find XOR subset
target = 2**214 - 9
M = Matrix(GF(2), [[int(b) for b in bin(h)[2:].zfill(215)] for _, h in pool])
t = vector(GF(2), [int(b) for b in bin(target)[2:].zfill(215)])
solution = M.solve_left(t)

chosen = [pool[i][0] for i in range(len(pool)) if solution[i] == 1]
# Send chosen strings to the server, then recover key:
# key_y = (hint1_y + hint2_y) * pow(2, -1, p) % p
```

*(A helper script is also provided in the challenge attachments.)*

### Step 2 — Test locally first

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → Verify it works before connecting to the server
```

### Step 3 — Connect and send the chosen strings

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42125
# → Send chosen strings → recover key → get flag
# → ictf{omg_xor_is_just_like_multiplication_chat}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Linear algebra over GF(2), elliptic curve arithmetic | ⭐⭐⭐ Hard |
| Python 3 | String generation, hashing, server interaction | ⭐⭐ Medium |
| `nc` | Connect to the challenge server | ⭐ Easy |

---

## Key Takeaways

- Prime orders of the form `2^k + c` have special XOR/negation relationships — always investigate them
- **Linear algebra over GF(2)** lets you find subsets that XOR to any target value
- Forcing a sign flip on an ECC scalar is a powerful trick to extract secret keys
- Always test your exploit locally before spamming the server!
