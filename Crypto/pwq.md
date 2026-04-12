# pwq — ImaginaryCTF Writeup

**Challenge:** pwq  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{generic_monthly_coppersmith_challenge}`

---

## Description

> pwq pwq pwq pwq pwq

---

## Background Knowledge (Read This First!)

### What is RSA and why does p, q matter?

**RSA** encryption relies on two secret large primes **p** and **q**. Their product `n = p * q` is public, but p and q stay hidden. Recovering p or q breaks RSA completely.

### What is `w` here?

The challenge leaks information **modulo w** — a special modulus smaller than p and q. You get:

- `(p + q) mod w` — the sum of the two primes, reduced
- `(p * q) mod w` — the product of the two primes, reduced (this is just `n mod w`)

### What is a Gröbner Basis?

A **Gröbner Basis** is an algebraic technique for solving systems of polynomial equations. Here we have:

- `x + y ≡ (p+q) mod w`
- `x * y ≡ (p*q) mod w`

Two equations, two unknowns (`x = p mod w`, `y = q mod w`). Feed this into SageMath's Gröbner Basis solver to find both values.

### What is Coppersmith's Method?

**Coppersmith's method** lets you find small roots of a polynomial modulo a large number. Since you know `p mod w` (the lower bits of p), you can set up:

```
f(x) = x * w + (p mod w)   ← equals p when x is the "upper part"
```

If the upper part of p is small relative to n, Coppersmith finds it efficiently.

---

## Solution

### Step 1 — Connect to the server and collect the leak

```
┌──(zham㉿kali)-[~]
└─$ nc 155.248.210.243 42124
# → Receive: n, (p+q) mod w, (p*q) mod w, ciphertext
```

### Step 2 — Create the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

Paste this:

```python
# SageMath — Gröbner Basis to get p mod w and q mod w
R.<x, y> = PolynomialRing(Zmod(w))
I = R.ideal([x + y - s,    # s = (p+q) mod w
             x * y - m])   # m = (p*q) mod w = n mod w
B = I.groebner_basis()
# Solve to get p_mod_w and q_mod_w

# Coppersmith to recover full p
P.<x> = PolynomialRing(Zmod(n))
f = x * w + p_mod_w
roots = f.small_roots(X=2^(512 - w.nbits()), beta=0.5)
p = int(roots[0]) * w + p_mod_w
q = n // p

# Decrypt the flag
from Crypto.Util.number import long_to_bytes
d    = pow(e, -1, (p-1)*(q-1))
flag = long_to_bytes(pow(c, d, n))
print(flag)
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → ictf{generic_monthly_coppersmith_challenge}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Gröbner Basis + Coppersmith `small_roots` | ⭐⭐⭐ Hard |
| Python 3 | RSA decryption | ⭐ Easy |
| `nc` | Connect to the challenge server | ⭐ Easy |

---

## Key Takeaways

- Leaking `(p+q) mod w` and `(p*q) mod w` together is **fatal** — they form a solvable system
- **Gröbner Basis** is the algebraic hammer for systems of modular polynomial equations
- **Coppersmith's method** recovers a full prime from a partial bit leak
- This class of challenge is extremely common in CTF crypto — learn it well!
