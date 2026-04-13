# apbq-rsa-iii — ImaginaryCTF Writeup

**Challenge:** apbq-rsa-iii  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{wh0_n33ds_0rtho_l4tt1c3_wh3n_y0u_c4n_us3_quadr4tic_f0rmu1a}`

---

## Description

> Since apbq-rsa-ii is easy these days, I updated it from v2 to v3!

**Attachment:** `output.txt` containing hints and RSA ciphertext

---

## Background Knowledge (Read This First!)

### What is apbq-RSA?

**apbq-RSA** is a variant of RSA where instead of giving you `n` and `e` directly, the server gives you **hints** — values derived from `a*p + b*q` (hence the name). The goal is to recover `p` and `q` from these hints to break RSA.

### What is the trick in version iii?

If you **divide any two hints**, you get a ratio `R` that satisfies:

```
(R - x)(R - y) ≡ 0  (mod n)
```

for some **small values** `x` and `y`. "Small" here means the ratio of two small numbers.

This turns the problem into **multivariate Coppersmith** — finding small roots of a system of polynomial equations modulo `n`.

### What is the Quadratic Formula doing here?

After setting up the right polynomial using the hints, the roots `c1` and `c2` can be found using the classic **quadratic formula** — hence the flag says "who needs ortho lattice when you can use quadratic formula!"

---

## Solution

### Step 1 — Download the output file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O output.txt
└─$ cat output.txt
# → Contains: n, c, hints (list of 3 values)
```

### Step 2 — Create the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

Paste this (provided by challenge author):

```python
from sage.all import *
from Crypto.Util.number import long_to_bytes
exec(open('output.txt').read())

h0, h1, h2 = vector(Zmod(n), hints)

# Dividing hints gives ratio R with small residues mod p and q
M = matrix([[h1/h0, h2/h0, 0],
            [h0/h1, 0,     h2/h1],
            [0,     h0/h2, h1/h2]])

# LLL reduction to find the small values x, y, z
x, y, _, z, *_ = block_matrix(ZZ, [[1, M], [0, n]]).LLL()[0]

# Quadratic formula to recover p
p = gcd([2*y*h0 - h1*(z + sqrt(z*z - 4*x*y))]).lift()

print(long_to_bytes(pow(c, pow(0x10001, -1, (p-1)*(n//p-1)), n)))
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → ictf{wh0_n33ds_0rtho_l4tt1c3_wh3n_y0u_c4n_us3_quadr4tic_f0rmu1a}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | LLL lattice reduction + quadratic formula to recover p | ⭐⭐⭐ Hard |
| Python `Crypto` | RSA decryption once p is known | ⭐ Easy |

---

## Key Takeaways

- Dividing two apbq hints creates a ratio with **small residues mod p and q** — the core insight
- **LLL lattice reduction** finds small vectors efficiently in high-dimensional spaces
- The **quadratic formula** can recover roots from the polynomial once LLL gives you the coefficients
- The flag itself is a joke at the expense of people who over-complicate it with orthogonal lattices
